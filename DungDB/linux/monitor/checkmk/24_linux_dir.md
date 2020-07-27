# Các thư mục liên quan trong giám sát Linux

`/usr/bin/check_mk_agent`:	Vị trí cài đặt checkmk

`/usr/lib/check_mk_agent`:	Thư mục home của agent extensions

`/usr/lib/check_mk_agent/plugins`:	Nơi đặt các plugins

`/usr/lib/check_mk_agent/local`:	Nơi đặt ‘local checks’ của user

`/var/lib/check_mk_agent`:	Nơi chứa dữ liệu của checkmk agent

`/var/lib/check_mk_agent/cache`:	Nơi chứa dữ liệu cache của các phiên.

`/var/lib/check_mk_agent/job`:	Nơi chứa dữ liệu job. Các file này sẽ được add vào output mỗi khi agent thực thi

`/var/lib/check_mk_agent/spool`:	Contains data created by e.g., cron jobs, and that includes its own section. These are also appended to the agent output.

`/etc/check_mk`:	Nơi chứa các file cấu hình của agents

`/etc/check_mk/mrpe.cfg`:	Configuration file for MRPE – for the execution of standard, Nagios-compatible check plug-ins.

`/etc/check_mk/encryption.cfg`:	Configuration for the encryption of the agent data.

`/etc/xinetd.d/check_mk_agent`:	Cấu hình cho xinetd, thứ kết nối các output của agent tới TCP-Port 6556.

`local/share/check_mk/agents/custom`:	The home directory for own files which are to be delivered with baked agents.