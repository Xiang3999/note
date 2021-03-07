# The Quick Sort

```nasm
DATASEG SEGMENT
	DATABASE DW 9991H,9999H,9996H,9993H,9992H,9998H,9994H,9990H,9997H,8888H
	NUM      DW 0014H
DATASEG ENDS
CODESEG SEGMENT
	ASSUME   CS : CODESEG, DS : DATASEG
	MAIN PROC FAR
	    START :		
						PUSH DS
						XOR  AX,AX
						PUSH AX
						
	           			MOV  AX,DATASEG  
						MOV  DS,AX	
	           
						MOV  SI, OFFSET DATABASE  ;EQUALENT: LEA SI DATABASE 
						MOV  DI, NUM
						SUB  DI, TYPE DATABASE    ;RIGHT 	BOUNDARY
						CALL QUICKSORT           
						CALL DISP
				        RET
	MAIN ENDP	
	QUICKSORT PROC NEAR
					    CMP  SI,DI
						JNL  END_QUICKSORT       ; CONTINUE WHEN LEFT BOUNDARY < RIGHT BOUNDRY
						PUSH DI                  ; develer right boundary parameter
						PUSH SI			 		 ; develer left  boundary parameter					 
						CALL QUICKPASS
      
						POP  DI             	 ; POP  divid BOUNDARY
						POP  SI		         	 ; POP  left  BOUNDARY
						PUSH DI                  ; PUSH divid BOUNDARY						
						SUB  DI, TYPE DATABASE   ; COPY LEFT-HALF MAX INDEX TO DI
						CMP  SI, DI
						JL   IF_TO
				    	JNL  ELSE_TO				
			    IF_TO:	
			    		CALL QUICKSORT           ; recursively call QuickSort 	
			    ELSE_TO:
			    		POP  SI 	             ; POP  divid BOUNDARY
						POP  DI                  ; POP  RIGHT BOUNDARY
						ADD  SI, TYPE DATABASE   ; COPY RIGHT-HALF MAX INDEX TO SI
						CMP  SI, DI
						JNL  END_QUICKSORT
						
						CALL QUICKSORT           ; recursively call QuickSort 
		END_QUICKSORT : 
						RET	         ; 43 Line
				
	QUICKSORT ENDP	

	QUICKPASS PROC NEAR 
						POP  BX    
						POP  SI
						POP  DI
						PUSH DI
						PUSH SI
						MOV  DX,WORD PTR[SI]   	; STORE FIRAT VALUE AS DIVID BOUNDARY

	    	LOOP_OUT :	
	    				CMP  SI,DI              ;50
						JNB  END_QUICKPASS	    ; CONTINUE WHEN LEFT BOUNDARY < RIGHT BOUNDRY

		    LOOP_IN1 :	
		    			CMP  SI,DI
						LAHF           		    ;LOAD FLAGS TO AH
						AND  AH,01H
						CMP  AH,01H
						JNE  IGNORE_LOOP_IN1	;57 Line			     
						CMP  DX,WORD PTR[DI]
						LAHF           		    ;LOAD FLAGS TO AH
						AND  AH,41H
						CMP  AH,0H  
						JZ   IGNORE_LOOP_IN1
						SUB  DI,TYPE DATABASE
						JMP  LOOP_IN1   

	   IGNORE_LOOP_IN1: 
	   					CMP  SI,DI
	   					JNL  LOOP_IN2             ;WHEN SI IS EQUAL TO DI ,SWAP DATA BETWWEN RAM IS MORE SLOWLY AND INVALID 
	   					MOV  AX, WORD PTR[DI]		
						MOV  [SI],AX

		      LOOP_IN2:	
		      			CMP  SI,DI
						LAHF           		      ;LOAD FLAGS TO AH
						AND  AH,01H
						CMP  AH,01H
						JNE  IGNORE_LOOP_IN2				     
						CMP  WORD PTR[SI],DX
						LAHF           		      ;LOAD FLAGS TO AH
						AND  AH,41H 
						CMP  AH,0H 
						JZ   IGNORE_LOOP_IN2
						ADD  SI,TYPE DATABASE
						JMP  LOOP_IN2
	   IGNORE_LOOP_IN2: 
	   					CMP  SI,DI                ;WHEN SI IS EQUAL TO DI ,SWAP DATA BETWWEN RAM IS MORE SLOWLY AND INVALID
	   					JNL  IGNORE_SWAP  
	   					MOV  AX,WORD PTR[SI] 
				        MOV  [DI],AX
		  IGNORE_SWAP:
						JMP  LOOP_OUT
		 END_QUICKPASS:	
		 				MOV  [SI],DX         	  ;FIRST VALUE STORED INTO DIVID ADRRESS			
					    PUSH SI	
					    PUSH BX		
					    RET
	QUICKPASS ENDP	
	DISP PROC NEAR
						MOV  BX, OFFSET DATABASE        ; INITIAL LOOP CONTROL VARIABLE
						
			LOOP_OUTER:
						CMP  BX, NUM
						JNB  END_OUTER_LOOP
						MOV  BP, SP
						MOV  AX, [BX]
			LOOP_IN:
						CMP  AX, 00H
						JE   LOOP_DISP
						MOV  DX, 0000H
						MOV  CX, 0AH 
						DIV  CX
						ADD  DL, 30H
						PUSH DX
						JMP LOOP_IN
						 
			LOOP_DISP:	
						CMP  SP,BP
						JNB  END_LOOP_DISP
						POP  DX
						MOV  AH, 02H
						INT  21H
						JMP  LOOP_DISP
		END_LOOP_DISP:
						MOV  DL, 20H         ;SPACE ASCII VALUE :20H
						MOV  AH, 02H
						INT  21H

						ADD  BX, TYPE DATABASE
						JMP  LOOP_OUTER
		END_OUTER_LOOP:			
						RET
	DISP ENDP
CODESEG ENDS
        END START
```



| Instructions | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| INT          | −Used to interrupt the program during execution and calling service specified. |
| MOV          | Used to copy the byte or word from the provided source to the provided destination. |
| PUSH         | Used to put a word at the top of the stack.                  |
| POP          | Used to get a word from the top of the stack to the provided location. |
| LAHF         | Used to load AH with the low byte of the flag register.      |
| ADD          | Used to add the provided byte to byte/word to word.          |
| SUB          | Used to subtract the byte from byte/word from word.          |
| CMP          | Used to compare 2 provided byte/word.                        |
| AND          | Used for adding each bit in a byte/word with the corresponding bit in another byte/word. |
| XOR          | Used to perform Exclusive-OR operation over each bit in a byte/word with the corresponding bit in another byte/word. |
| JMP          | Used to jump to the provided address to proceed to the next instruction. |
| JZ           | Used to jump if equal/zero flag ZF = 1                       |
| JNE          | Used to jump if less than/equal/if not greater than instruction satisfies. |
| JNL          | Used to jump if greater than/equal/not less than instruction satisfies. |
| JNB          | Used to jump if above/not below instruction satisfies.       |
| RET          | Used to return from the procedure to the main program.       |
| LOOP         | Used to loop a group of instructions until the condition satisfies, i.e., CX = 0 |





