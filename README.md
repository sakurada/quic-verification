# ProVerif script for formal verification of the QUIC protocol

This archive contains the ProVerif script for analyzing the QACCE security
of the QUIC protocol. This result was first presented at [1]. 
The script contanied here is the updated version.

In our analysis, we divide the QACCE security into a number of properties
and separately analyze them using ProVerif. To avoid redundancy, we have
only one ProVerif script, from which the scripts for those properties are
generated. To generate and execute the scripts, you need the GNU C
preprocessor (cpp) and the ProVerif tool to do this, and follow the
instructions below.


(I) To generate the scripts, run cpp as follows:

   cpp -P -w $CPPOPTS quic.pv > script.pv

where you must replace $CPPOPTS with options as follows.

You must choose exactly one of the following options below, depending the
security properties you analyze:

 1) Security against the server-impersionation attack: -DSERVER_IMPERSONATION
 2) Authenticity in the channel-corruption attack:     -DAUTHENTICITY
 3) Secrecy in the channel-corruption attack:          -DSECRECY
 4) Security against the IP spoofing attack:           -DIP_SPOOFING

If you want to analyze the fixed version of QACCE security, add the option
'-DFIXED'.

To reduce time for the analysis, add the cpp option "-DNO_DH_COMM" such as:
   cpp -P -w -DIP_SPOOFING -DNO_DH_COMM quic.pv > script.pv
This omits the equation for commutativity of groups in the generated script.

For example, if you want to analyze the security against the IP spoofing
attack with the fixed version of the security definition, generate the
script as follows:
  
  cpp -P -w -DIP_SPOOFING -DFIXED quic.pv > script.pv

(II) To use a generated script, run proverif as follows:

   proverif script.pv

To read the output, please read the ProVerif manual.

For the original script, the times comsumed for the analyses on the authors' computer
(with Intel Xeon E5-2640 v3 @ 2.60GHz and 256GB of memory running CentOS 7.2)
are as follows:

  1)   6[min] (  8[min] with -DFIXED)
  2)   6[min] (  5[min] with -DFIXED)
  3)  52[min] ( 51[min] with -DFIXED)
  4) 569[min] (275[min] with -DFIXED),
  4') with -DNO_DH_COMM (ommitting associativity)
       7[min] (  5[min] with -DFIXED))

[1]   Sakurada, H., Yoneyama, K., Hanatani, Y., Yoshida, M. (2016).
  Analyzing and Fixing the QACCE Security of QUIC.
  In: Chen, L., McGrew, D., Mitchell, C. (eds) Security 
  Standardisation Research. SSR 2016.
  Lecture Notes in Computer Science(), vol 10074. Springer, Cham.
  https://doi.org/10.1007/978-3-319-49100-4_1
