(C) 2013  AG1LE Mauri Niininen 

This software is a Morse decoder orinally created by Dr. E. L. Bell in 1977.  
The software was manually entered from Fortran program listings and later converted to C++
by AG1LE Mauri Niininen.  

2013-SEP-29 
	

2013-SEP-26 
	enabled decoding from FLDIGI data feed. clamping x value to 1.0 max as FLDIGI sends values over 20.0 during startup before AGC kicks in.
	BUG: P(dah) abruptly goes from near 1.0 down and the bounces back => P(max) jumps to huge values 
	too long dahs?  
	
2013-SEP-25 
	BUG: missing word space /pause between words QUICK BROWN  when < 30 db SNR ?
	FOUND: enabled  noise.c processing in morse.c 
	changed to: 
			noise_(&x, &rn, &zout);
			retstat = proces_(&zout, &rn, &xhat, &px, &elmhat, &spdhat, &imax, &pmax);
	much better decoding with low SNR test signals.

2013-SEP-25  
	run "./morse t test/test20db.in | less"
	BUG: QUICD  and FOB  when high 20 dB SNR?  
	D should be K  and  B should be X 
	for some reason last 'dah' following word space gets decoded as 'dit'.
	FOUND:  Initl.c - line 123    1, 1, 0, 0, 0, 0,   // mauri 2013-09-25 bugfix
	had 0 instead of 1 in state k=4 
	

2013-SEP-02	Morse decoding works on C++ version. Added decoding struct TREE in transl.c 
			and logic to translate incoming morse symbols. This is marked as version v01.


2013-SEP-01  	Initial version. Original Fortran sources compiled with  
			gfortran -g  *.f 
		produces a.out  executable program.  

		Based on initial testing the element state estimation works, but translating letters 
		has still problems. Produces a sequence of letter states but translation to actual 
		characters produces incorrect letters. 

 		Compilation of C-sources produced by f2c is done with following commands:

			gcc -c *.c 
			gcc  *.o -lf2c -lm

		produces a.out executable program. The output of both versions of a.out is in 
			output_c.txt
			output_f.txt 

		These have minor differences - source still unknown. 
