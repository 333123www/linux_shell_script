#!/usr/bin/expect

set ip [lindex $argv 0]
set pass [lindex $argv 1]
set passn [lindex $argv 2]
set timeout 5
spawn ssh $ip
expect {
	"yes/no" { send "yes\n";exp_continue; }
	"*password*" { send "$pass\n" }		
	}
expect "*@*" { send "echo '$passn' | passwd --stdin root\n" }
interact
