~~~~~~~~~~~~~~~~

//***FILE 122 CONTAINS THE RMSG SUB-SYSTEM AND SOME JES2 EXITS      *   FILE 122
//*            USED AT ALLERGAN INC OF IRVINE CALIFORNIA.  THIS     *   FILE 122
//*            FILE IS IN IEBUPDTE SYSIN FORMAT,  FOR ADDITIONAL    *   FILE 122
//*            INFORMATION SEE THE MEMBER CALLED $$DOC              *   FILE 122
//*                                                                 *   FILE 122
//*            THE RMSG SUB-SYSTEM IS AN 'AUTOMATIC OPERATOR'       *   FILE 122
//*            SYSTEM THAT MONITORS AND REACTS TO SELECTED          *   FILE 122
//*            CONSOLE TRAFFIC AND USER WRITTEN COMMANDS.           *   FILE 122
//*                                                                 *   FILE 122
//*            THE RMSG SUB-SYSTEM RUNS ON MVS/SP AND MVS/XA        *   FILE 122
//*            WITHOUT ANY MODIFICATION.  THE J2SRB01 ROUTINE RUNS  *   FILE 122
//*            ON MVS/SP AND MVS/XA WITHOUT ANY MODIFICATION.       *   FILE 122
//*                                                                 *   FILE 122
//*                                                                 *   FILE 122
//*             MEMBER              DESCRIPTION                     *   FILE 122
//*                                                                 *   FILE 122
//*            CMDRMSG  SAMPLE MVS STARTUP COMMANDS ISSUED BY       *   FILE 122
//*                     RMSGLOAD                                    *   FILE 122
//*                      PLACE THIS MEMBER IN SYS1.PARMLIB.  THIS   *   FILE 122
//*                      IS A LIST OF COMMANDS THAT ARE ISSUED BY   *   FILE 122
//*                      RMSGLOAD AFTER RMSG IS INITIALIZED.  SEE   *   FILE 122
//*                      QUITMVS FOR SHUTDOWN COMMANDS THE PROC     *   FILE 122
//*                      RMSGLOAD REFERS TO THIS MEMBER             *   FILE 122
//*                                                                 *   FILE 122
//*            COMEIN   ENTRY MACRO FOR SOME ROUTINES               *   FILE 122
//*                      PLACE THIS MEMBER IN YOUR USER MACLIB      *   FILE 122
//*                                                                 *   FILE 122
//*            GETOUT   EXIT MACRO FOR SOME ROUTINES                *   FILE 122
//*                      PLACE THIS MEMBER IN YOUR USER MACLIB      *   FILE 122
//*                                                                 *   FILE 122
//*            IEFSSN00 SAMPLE SUB-SYSTEM NAME TABLE                *   FILE 122
//*                      ADD AN ENTRY FOR "RMSG" TO YOUR            *   FILE 122
//*                      SUB-SYSTEM NAME TABLE IN SYS1.PARMLIB.     *   FILE 122
//*                                                                 *   FILE 122
//*            JES2PARM SAMPLE JES2 PARMS                           *   FILE 122
//*                      THESE JES2 PARMS ACTIVATE ALL OF OUR       *   FILE 122
//*                      JES2 EXITS AND STARTS A NJE/NJI LINK       *   FILE 122
//*                      BETWEEN MVS AND VM.  BEWARE OF THE         *   FILE 122
//*                      VIRTUAL PRINTER NUMBERS. THEY ARE          *   FILE 122
//*                      CRITICAL BEWARE OF THE NJE NODE NAMES.     *   FILE 122
//*                                                                 *   FILE 122
//*            J2SRB01  SRB TO CLOSE VIRTUAL PRINTERS               *   FILE 122
//*                      THIS SRB ISSUES A DIAGNOSE 8 TO CLOSE      *   FILE 122
//*                      VIRTUAL PRINTERS.  IT IS LOADED BY         *   FILE 122
//*                      RMSGLOAD AND ACTIVATED BY J2XIT01.  THE    *   FILE 122
//*                      LOAD MODULE MUST RESIDE IN THE SAME        *   FILE 122
//*                      LINKLIB AS RMSG.  SEE THE //LOADLIB DD     *   FILE 122
//*                      IN THE RMSGSUB PROC.  THE SSVT FOR RMSG    *   FILE 122
//*                      IS ALSO THE ANCHOR FOR J2SRB01             *   FILE 122
//*                                                                 *   FILE 122
//*            J2TBL03  ACCOUNT NUMBER TABLE FOR J2XIT03 ACCOUNT    *   FILE 122
//*                      NUMBER VALIDATION ROUTINE FOR BOTH MVS     *   FILE 122
//*                      AND CMS.  THIS ROUTINE IS LOADED AND       *   FILE 122
//*                      CALLED BY J2XIT03.  THIS ROUTINE, WHEN     *   FILE 122
//*                      ASSEMBLED UNDER CMS, CAN BE USED TO        *   FILE 122
//*                      VALIDATE ACCOUNT NUMBERS.                  *   FILE 122
//*                                                                 *   FILE 122
//*            J2XIT01  JES2 EXIT 1 TO CLOSE VIRTURAL PRINTERS      *   FILE 122
//*                      THIS ROUTINE KNOWS, BY PRINTER NUMBER,     *   FILE 122
//*                      WHICH PRINTERS ARE VIRTUAL PRINTERS.  NO   *   FILE 122
//*                      ACTION IS TAKEN FOR REAL PRINTERS.  IF     *   FILE 122
//*                      THE ENTRY IS FOR A START BANNER PAGE, A    *   FILE 122
//*                      1 LINE BANNER PAGE IS CREATED.  IF THE     *   FILE 122
//*                      ENTRY IS FOR A ENDING BANNER PAGE, THEN    *   FILE 122
//*                      THE PRINTER ADDRESS (CUU) AND OTHER        *   FILE 122
//*                      INFORMATION IS FORMATTED FOR J2SRB01 AND   *   FILE 122
//*                      J2SRB01 IS CALLED TO SCHEDULE A SRB TO     *   FILE 122
//*                      CLOSE THE PRINTER.  ENDING BANNER PAGES    *   FILE 122
//*                      ARE NOT PRODUCED FOR VIRTUAL PRINTERS.     *   FILE 122
//*                                                                 *   FILE 122
//*            J2XIT02  JES2 EXIT 2 TO MODIFY JOB CARD AND INSERT   *   FILE 122
//*                     /*ROUTE CARD.                               *   FILE 122
//*                      1) CHECK FOR STARTED TASKS AND INSERT AN   *   FILE 122
//*                         ACCOUNT NUMBER IN THE STC JOB CARD.     *   FILE 122
//*                      2) INSERT A /*ROUTE CARD IF THE JOB CAME   *   FILE 122
//*                         FROM THE NJE/NJI LINK.                  *   FILE 122
//*                                                                 *   FILE 122
//*            J2XIT03  JES2 EXIT 3 TO VALIDATE ACCOUNT NUMBERS     *   FILE 122
//*                      THIS ROUTINE LOADS J2TBL03 TO VALIDATE     *   FILE 122
//*                      ACCOUNT NUMBERS.  SELECTED JOB NUMBERS     *   FILE 122
//*                      (SEE THE CODE) WILL CAUSE J2TBL03 TO BE    *   FILE 122
//*                      REFRESHED (RE-LOADED) OR INACTIVATED.      *   FILE 122
//*                                                                 *   FILE 122
//*            J2XIT04  JES2 EXIT 3 TO MODIFY JCL                   *   FILE 122
//*                      THIS ROUTINE COMMENTS OUT JOBCAT AND       *   FILE 122
//*                      STEPCAT CARDS FOR SELECTED JOB CLASSES.    *   FILE 122
//*                      IT ALSO ADDS SOME 'OUTPUT' CARDS TO        *   FILE 122
//*                      ROUTE THE JOBLOG ETC TO THE LOCAL NODE.    *   FILE 122
//*                                                                 *   FILE 122
//*            J2XIT09  JES2 EXIT 9 TO ENFORCE OUTPUT EXCESSION     *   FILE 122
//*                     FOR TEST JOBS                               *   FILE 122
//*                      THIS EXIT WILL ALLOW OUTPUT EXCESSION      *   FILE 122
//*                      FOR PRODUCTION JOBS.  TEST JOBS WILL       *   FILE 122
//*                      ABEND WHEN OUTPUT EXCESSION OCCURS.        *   FILE 122
//*                                                                 *   FILE 122
//*            QUITMVS  A LIST OF COMMANDS TO SHUT MVS DOWN BEFORE  *   FILE 122
//*                     AN IPL                                      *   FILE 122
//*                      PLACE THIS MEMBER IN SYS1.PARMLIB.  WHEN   *   FILE 122
//*                      THE OPERATOR ISSUES THE COMMAND 'QUIT      *   FILE 122
//*                      MVS' THESE COMMANDS WILL BE PUT ON THE     *   FILE 122
//*                      INTRDR BY RMSG.                            *   FILE 122
//*                                                                 *   FILE 122
//*            RCMD     ISSUE SELECTED JES2 COMMANDS                *   FILE 122
//*                      WE DON'T WANT OUR PROGRAMMERS ISSUING      *   FILE 122
//*                      JES2 OR OPERATOR COMMANDS.                 *   FILE 122
//*                       RCMD IS USED TO SEND A REQUEST TO RMSG    *   FILE 122
//*                       TO ISSUE SELECTED COMMANDS.  RCMD IS      *   FILE 122
//*                       PARM DRIVEN AND WILL ISSUE THE            *   FILE 122
//*                       FOLLOWING COMMANDS.                       *   FILE 122
//*                                                                 *   FILE 122
//*             PARM          COMMAND                               *   FILE 122
//*             SUPRA         $TI10,V        CHANGE INITIATOR CLASS *   FILE 122
//*                           $SI10          START THE INITIATOR.   *   FILE 122
//*             RLSE JOBNAME  $A'JOBNAME'    RELEASE A HELD JOB     *   FILE 122
//*             REFRESH       F LLA,REFRESH  REFRESH THE LLA FOR XA *   FILE 122
//*                                                                 *   FILE 122
//*                        * THE PRODUCTION CONTROL GROUP LINKS     *   FILE 122
//*                          ALL PRODUCTION PROGRAMS INTO A         *   FILE 122
//*                          LINKLIST DATASET.  RCMD WITH THE       *   FILE 122
//*                          REFRESH PARM IS THE LAST STEP OF THE   *   FILE 122
//*                          LKED JOB.                              *   FILE 122
//*                                                                 *   FILE 122
//*                        * IF THE F LLA,REFRESH COMMAND LOOKS A   *   FILE 122
//*                          LITTLE STRANGE, IT IS BECAUSE WE       *   FILE 122
//*                          HAVE MSX IN HOUSE AND ISSUE THE        *   FILE 122
//*                          COMMAND ON ALL PROCESSORS.  REMOVE     *   FILE 122
//*                          THE '¬ALL' AND THE COMMAND SHOULD      *   FILE 122
//*                          WORK FINE.                             *   FILE 122
//*                                                                 *   FILE 122
//*                      EXAMINE THE CODE FOR ADDITIONAL FEATURES.  *   FILE 122
//*                                                                 *   FILE 122
//*            RMSG     AUTOMATIC OPERATOR SUB-SYSTEM               *   FILE 122
//*                      RMSG IS A SUB-SYSTEM THAT MONITORS ALL     *   FILE 122
//*                      CONSOLE TRAFFIC AND REACTS TO SELECTED     *   FILE 122
//*                      MESSAGES AND COMMANDS.  THE SSVT FOR RMSG  *   FILE 122
//*                      IS ALSO THE ANCHOR FOR J2SRB01 RMSG        *   FILE 122
//*                      CONTAINS THE FOLLOWING FEATURES:           *   FILE 122
//*                      1) REPLY TO SELECTED WTOR MESSAGES.        *   FILE 122
//*                         EX: REPLY 'NOHOLD' TO THE REPLY HOLD    *   FILE 122
//*                             OR NOHOLD MSG.                      *   FILE 122
//*                      2) RESPOND TO SELECTED WTO MESSAGES -      *   FILE 122
//*                         EX: WHEN RMSG SEES  THE 'VTAM ACTIVE'   *   FILE 122
//*                             MSG IT WILL START TSO.              *   FILE 122
//*                      3) ALLOW USER COMMANDS.                    *   FILE 122
//*                         EX: THE COMMAND 'QUIT MVS' WILL ISSUE A *   FILE 122
//*                             SERIES OF COMMANDS TO SHUT DOWN MVS *   FILE 122
//*                             (SEE QUITMVS MEMBER).  JES2 WILL BE *   FILE 122
//*                             STOPPED AND A Z EOD WILL BE ISSUED. *   FILE 122
//*                                                                 *   FILE 122
//*            RMSGCMD  PROC USED BY RMSG TO WRITE COMMANDS TO THE  *   FILE 122
//*                     INTRDR                                      *   FILE 122
//*                      PLACE THIS MEMBER IN A PROCLIB             *   FILE 122
//*                                                                 *   FILE 122
//*            RMSGLOAD INITIALIZE RMSG AND LOAD J2SRB01 THIS       *   FILE 122
//*                     ROUTINE INITIALIZES THE RMSG SUB-SYSTEM     *   FILE 122
//*                     AND LOADS THE J2SRB01 ROUTINE.  IT ALSO     *   FILE 122
//*                     READS THE CMDRMSG MEMBER OF SYS1.PARMLIB    *   FILE 122
//*                     AND PUTS THE COMMANDS ON THE INTRDR.        *   FILE 122
//*                                                                 *   FILE 122
//*            RMSGSUB  PROC TO RUN RMSGLOAD AND INITIALIZE RMSG    *   FILE 122
//*                     SUB-SYSTEM                                  *   FILE 122
//*                      PLACE THIS MEMBER IN A PROCLIB             *   FILE 122
//*                      POINT TO THE LINKLIB THAT CONTAINS BOTH    *   FILE 122
//*                      RMSG AND J2SRB01.                          *   FILE 122
//*                      TO START RMSG ENTER THE COMMAND 'S RMSGSUB'*   FILE 122
//*                       WE PUT THIS COMMAND IN CMD00.             *   FILE 122
//*                      TO REFRESH RMSG AND J2SRB01 ENTER THE      *   FILE 122
//*                      COMMAND:                                   *   FILE 122
//*                      'S RMSGSUB,OPTION=FORCE'.                  *   FILE 122
//*                                                                 *   FILE 122
~~~~~~~~~~~~~~~~

