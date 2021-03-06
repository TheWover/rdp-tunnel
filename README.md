rdp2tcp lets you tunnel TCP traffic over an RDP Virtual Channel.  It includes port forwards, reverse port forwards, and a redimentary SOCKS5 proxy.  It does this by redirecting a local named pipe to the terminal services client. 

I got tired of having to compile the tool, so this is pre-compiled using rdesktop 1.8.3

To run it, launch the included rdesktop client with:
	./rdesktop -r addin:rdp2tcp:./rdp2tcp <ipaddress>

You should see:
	controller listening on 127.0.0.1:8477
	virtual channel disconnected

Upload rdp2tcp.exe to the Terminal Server. try copy/pasting the binary into a local wordpad document, opening wordpad on the terminal server, then copy/pasting the OLE object.  This has a fairly high success rate. 
if this isn't possible, you can try to use rdpupload. This will use sendkeys() to the active window (which should be rdesktop) to generate a vbscript file that'll write the EXE to disk.  Start notepad on the Terminal Server, then run this on the client:
	./rdpupload -x -f vb rdp2tcp.exe - | xte
Then give the rdesktop window focus.  This takes FOREVER.

Once you get rdp2tcp.exe on the Terminal Server, run it. You should see this on your rdesktop client logs:
	chan            < 6       
	virtual channel connected
and this on the Terminal Server:
	chan			< 6
	channel connected

To add port forwards, use the rdp2tcp.py script.
	Straight Port Forward
	./rdp2tcp.py add forward <lhost> <lport> <rhost> <rport>

	Reverse Port Forward
	./rdp2tcp.py add reverse <lhost> <lport> <rhost> <rport>

	Bind a remote process to a local port (like a cmd.exe bindshell)
	./rdp2tcp.py add process <lhost> <lport> <process>

	SOCKS5 Proxy (very basic. advanced stuff not supported)
	./rdp2tcp.py socks5 <lhost> <lport>

	Run a shell command on the Terminal Server in 'cmd /c'
	./rdp2tcp.py sh <command>

 
