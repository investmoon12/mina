Mina Protocol (MINA) Staking Guide
There are two ways to staking right now.
Web wallet
Command line (cli)

All the new mina wallet now present like Clor.io and others , these are not yet audited so i personally wont recommend them to use. though you can see how to delegate using Clor.io here : https://medium.com/everstake/how-to-delegate-mina-mina-protocol-via-clorio-wallet-b35a11ddbc39
our public key for delegation is B62qns9cPvDwckhJXHpWZZ8b8T8oUgoF4Enpax5zNVBYYMtQwHf4Cmp
Lets see from beginning how can you use cli to delegate mina its pretty easy and safe. i have described very little things here because this guide meant for beginners.
1. Getting a server and setting it up for installing mina software.
we will use Google cloud. you can get a free $300 in Google cloud here https://cloud.google.com/free . you sign up and add your credit card and you get $300 credit simple as that . you can use this video to know how to create a free google cloud account here :https://www.youtube.com/watch?v=iZgD6p0slTU .
Now assuming you have a free $300 credit in you gcp(google cloud platform) lets set the firewall first .we have to allow inward ports 8302 8303 and 22. and outward 8302 8303. check the video for detail.

Now let us create a instance
here we will select 8 vcpu and 32 gb for the instance . we will use Ubuntu 18.04. Check the video for detail

Now let us log in to our instance and install the software for mina. lets log in in to ssh and put these commands one by one as in video
sudo -i 
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.1.5-a42bdee
apt install supervisor

Now lets set up wallet
put this commands in to cli as show in video . you have to provide password for the wallet in the process. also you can copy your public key . i have shown in video . To that address you have to send your mina tokens after this step .
sudo apt-get install mina-generate-keypair=0.2.12-718eba4
mkdir ~/keys
chmod 700 ~/keys
mina-generate-keypair -privkey-path ~/keys/my-wallet

after this you must download the wallet to your local drive and save it before transferring mina to the wallet. follow this YouTube video https://www.youtube.com/watch?v=C6YmQYf3WBA to set up filezilla and download the keys folder.
Now we have set up wallet installed mina software lets start the daemon.we here using supervisor for starting daemon. we have already installed supervisor. now lets config it. enter these commands in your cli and a blank window will appear
nano /etc/supervisor/conf.d/mina.conf
you have to put these commands in to the window and press ctl + x then y then enter.
[program:mina]
command=  mina daemon --peer-list-url https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt --block-producer-key /root/keys/my-wallet --block-producer-password "jenni" --generate-genesis-proof true
autostart=true
autorestart=true
stderr_logfile=/var/log/mina.err.log
stdout_logfile=/var/log/mina.out.log
supervisorctl update
please note that i have used password as jenni for the wallet creation .

now after 10 minutes lets check the daemon status . the status should be synced . if not wait a bit .
mina client status 

Lets do the delegation now.
for delegation you have to put these commands in cli
mina accounts import -privkey-path ~/keys/my-wallet
mina accounts unlock -public-key  put your public key here

now the command for delegation
mina client delegate-stake \
    -receiver B62qns9cPvDwckhJXHpWZZ8b8T8oUgoF4Enpax5zNVBYYMtQwHf4Cmp \
    -sender put your public key \
    -fee 0.1
and just hit enter. before that make sere you have added mina to you wallet. you can check you balance in cli or in mina explorer by https://minaexplorer.com/wallet/yourwallet_address
lets see the video .

Do not forget to back up your wallet .
