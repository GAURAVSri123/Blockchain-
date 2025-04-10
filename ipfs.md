<b> IPFS Account</b> <br>
![Image](https://github.com/user-attachments/assets/4a61a62a-2ecf-4eb1-9dec-d6cd78ca62d3) <br>

<b> IPFS Uploaded files interface : </b>
![Image](https://github.com/user-attachments/assets/7749bfb0-faa0-45b1-9020-716f96c56646) <BR>

<b> IPFS Daemon Command </B>
![Image](https://github.com/user-attachments/assets/98630418-284c-441d-b187-681de0c31b6e) <br>

<B> IPFS file encrypted command </b>
![Image](https://github.com/user-attachments/assets/0a79052d-c704-4774-8c8c-3168eb4439e6) <br>

<IPFS wsl/linux Commannd which help to open the ipfs </b> <br>
Installation of IPFS on local machine. Further, upload the files (such as photos, audio, and video) on IPFS and share it with other through content identifier (i.e., hash). Perform assessment using ubuntu WSL.
Commands (IPFS)
1.	Install the IPFS through WSL: wget https://dist.ipfs.tech/kubo/v0.32.1/kubo_v0.32.1_linux-amd64.tar.gz 
Or 
wget https://github.com/ipfs/kubo/releases/download/v0.32.1/kubo_v0.32.1_linux-amd64.tar.gz
2.	tar -xvzf kubo_v0.32.1_linux-amd64.tar.gz
3.	Kubo is a reference implementation of IPFS in Go: cd kubo 
4.	ls
5.	sudo bash install.sh
6.	ipfs –version
7.	ipfs init
8.	ipfs daemon
9.	On Browser: http://127.0.0.1:5001/webui
10.	To run ipfs daemon in background: ipfs daemon > ipfs.log 2>&1 &
11.	This is daemon ID: [1] 772
12.	Add file: echo "Hello, IPFS!" > hello.txt
13.	ipfs add hello.txt
14.	ipfs cat <CID>
15.	Add a directory: 
mkdir myfolder
echo "File 1 content" > myfolder/file1.txt
echo "File 2 content" > myfolder/file2.txt
16.	ipfs add -r myfolder
17.	ps aux | grep ipfs
18.	kill <PID>
19.	in Browser you can directly run this to see the IPFS: https://ipfs.io/ipfs/QmWd9cavD8UGZQcqYBVhZqs2Jure5W9cgxR7S6TC4StfZe


<b> ipfs file privacy and encryption commands </b> <br>
Perform encryption and privacy on IPFS through command line.  
IPFS privacy and encryption
1.	echo "Hello, IPFS!" > myfile.txt
2.	ipfs add myfile.txt
3.	openssl enc -aes-256-cbc -pbkdf2 -iter 100000 -salt -in myfile.txt -out myfile_encrypted.txt -pass pass:yourpassword
4.	ipfs add myfile_encrypted.txt
5.	cat myfile_encrypted.txt
6.	openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000 -in myfile_encrypted.txt -out decrypted_file.txt -pass pass:yourpassword
7.	cat decrypted_file.txt
8.	ipfs add decrypted_file.txt





