# oorexx
oorexx to get/put files from z/OS
/*REXX-----------------------------------------------

Ernesto Fragoso Luna.
=====================
Envia un archivo via ftp.
-----------------------------------------------------*/
/*
trace ?r
*/

host   = '172.24.48.141'   /*DB2B              */
uid    = YYY
pwd    = XXX
jclin  = 'MVSELF4.PRIVLIB.JCL(BINDREX3)'
jclout = 'C:\Mainframe\Bindout.txt'

server    = '172.24.48.141'   /*DB2B              */
userid    = YYY
passwd    = 'XXX
trclog    = "C:\eclipse\Rexx\RexxExecutables\zOS\logs\logfile.txt"
LocalFile = "C:\eclipse\Rexx\FilesToReceive\Getsample.rex"
RemoteFile = "sample.put"
rc     = 0

say 'Iniciando el programa ftprex01.rex'
say '=================================='
say date() time()
say ' Local File   = 'LocalFile
say ' Remote File  = 'RemoteFile
say '=================================='
say ' '

myftp  = .rxftp~new()       

/*-----------------------------------------------------
 Start tracing FTP commands and logging of replies   
 ------------------------------------------------------*/
rc = myftp~FtpTrace()
rc = myftp~FtpTraceLog(trclog,)
If rc = 0 then 
  Say " Replies will be written to log file: "trclog"."
Else 
  Say " No writing to log file: "trclog" possible."   

/*------------------------------------------------------
 Define remote host and user to be used during the session
 -------------------------------------------------------*/
rc = myftp~FtpSetUser(server, userid, passwd)
If rc = 0 then 
  Say " Connection established."
Else 
  Call Terminate " Connection failed."

/*------------------------------------------------------
  Transfer an ASCII file to the remote ftp server
  ------------------------------------------------------*/
/*
rc = myftp~FtpPut("C:\eclipse\Rexx\RexxSource\zOS\sample.rex", "sample.put", "ASCII")
*/  
rc = myftp~FtpGet(LocalFile,RemoteFile, "ASCII")
If rc = 0 then 
  Call Terminate " File has been recived. "
Else 
  Call Terminate " File has NOT been recived."

/*------------------------------------------------------
 Terminate the file transfer 
 -------------------------------------------------------*/
Terminate:
Parse Arg Message
Say Message
rc = myftp~FtpLogoff()
rc = myftp~FtpTraceLogoff()
rc = myftp~FtpTrace()
exit rc

/*
IF RxFuncQuery('FtpDropFuncs') \= 0 THEN DO
  IF RxFuncAdd('FtpLoadFuncs','rxftp','FtpLoadFuncs') = 0 THEN
    CALL FtpLoadFuncs
END

CALL FtpTraceLog        ftpout, '1'
CALL FtpSetUser         host, uid, pwd
CALL FtpSetActiveMode   '1'
CALL FtpSite            'filetype=JES'
CALL FtpSite            'jesrecfm=F'
CALL FtpSite            'jeslrecl=80'
CALL FtpGet             jclOut, jclIn, 'ASCII'
CALL FtpLogoff
CALL FtpTraceLogOff
*/


::requires "rxftp.cls"
