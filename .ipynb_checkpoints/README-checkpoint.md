# Blockchain_Simulation

This is a tutorial/how-to guide on creating your own private blockchain netwrok using a proof-of-authority setup. This will be explained for the use of windows and will require you to download two programs. One called geth or Go Ethereum and the other is called MyCrypto. This tutorial also assumes that you have GitBash installed on your system as this will be needed in order to create the blockchain network.

## Installations

Go to https://geth.ethereum.org/downloads/ and download the Geth & Tools 1.9.22 for Windows making sure to install either the 32-bit or 64-bit version depending on your PC.

Next you will need to go to https://download.mycrypto.com/ and download MyCrypto. It should automatically detect that you are a Windows user, but double check and make sure you are downloading the Windows version.

## Creating the Blockchain Network

Once you have downloaded Geth & Tools it should show a zipped file you will need to extract the files from this folder. The folder, once extracted, should have a name similar to geth-alltools-windows-amd64-1.9.22-c71a7e26. Inside this folder there will be another folder with a very similar name. We want to change the name of this folder to Blockchain-Tools. If you don't like this name then pick one you do like, but change the name as it will make it easier to locate in GitBash. You will then need to copy this newly named folder and paste it into an easy to navigate to folder. I recommend either the Downloads folder or the Documents folder. Once you have re-named the folder and pasted it to your desired location we can open Git Bash.

Once GitBash is open navigate to your newly created folder, For our purposes we re-named the folder Blockchain-Tools and pasted it into our Downloads folder so in GitBash I run the following command:

cd Downloads

cd Blockchain-Tools

Once here I run the following command

./geth account new --datadir node1

This will generate an account we need in order to complete the mining on our network and in fact we will be creating two accounts with this one being the first.

The account will generate and it will ask for a password. The password we used is testner0123. It doesn't have to be anything long and complicated and you will want to write it down or store it someplace that you can easily find. It will ask you to re-type the password it will generate a public address key and a path of the secret key file. Copy and store both of these in a safe place. It is okay to share the public address, but keep the secret key safe and don't share it with anyone.

Now we will run this command:

./geth account new --datadir node2

Like before it will ask for a password, you can use the same password as before or a different one if you wish. Once you have entered and re-entered a password a public address and secret key file will be generated. Copy paste both of these and store them in a safe place. I recommend using a text editor such as Notepad++ to store your information.

Once this is done, open a new GitBash and navigate to the Blockchain-Tools folder as before. You can do this in the same GitBash Window, I just personally like to be able to reference back to things if need be. Once you have navigated to the Blockchain-Tools folder in a new GitBash window run the following command:

./puppeth

This will start the puppeth program and ask you want to name your network, thus the beginning of creating or own private blockchain network. For our purposes we called the network auriel, which is a nod to the unique weapon in the Skyrim: Dawnguard DLC Auriel's Bow. You can name your network anything you want. I personally enjoy Epic fantasy novels and Skyrim is one of my favorite games, but I would say pick something you enjoy or something from an area that you enjoy for fun.

After we enter a name it will generate 4 options, we will select option 2, configure new genesis.

On the next question we will choose option 1, create new genesis from scratch.

We will then select option 2, Clique - proof-of-authority.

Next we designate that block time should be 5 seconds be entering the number 5.

It then asks which accounts are allowed to seal. Here we will copy and paste the first account we gereated previously without the 0x that is at the beginning, press enter, copy and paste the second account we generated, again without the 0x at the begiing, and then press enter twice to get to the next question.

The next question asks which accounts should be pre-founded, we will repeat the previous step, pasting the first account, pressing enter, the second account, and then pressing enter twice, both without the 0x that is at the beginning.

We will enter no to the next question and then type in a 3 digit chain ID. I recommend skipping to the part about setting up our custom network in MyCrypto and testing your Chain ID to ensure it is not already being used by a public network. If you do not double check and enter an ID that is already taken then you will have to begin this tutorial over, from scratch.

After we enter our Chain Id, it will again give us 4 options. We will select option 2 again to manage our existing genesis that we just created. We will then select option 2, Export genesis configurations. Hit enter for the next question and the files will be created into the Blockchain-Tools folder.

You can reference the following two screenshots to see if you did it correctly.

## Initiating JSON and Start Mining

Now that our network has been created you can either enter the Ctrl + C command to exit puppeth, or if you are like me and want to reference back to things, you can open a new GitBash window and navigate again to the Blockchain-Tools folder. Either way we will now want to run the following command.

./geth init "name of json file" --datadir node1

For our purposes it would look like this:

./geth init auriel.json --datadir node1

The name of the json file can be found by opening up a file explore and navigating to the Blockchain-Tools folder.

We will run this command again for node2

./geth init auriel.json node2

If successful it should say Successfully wrote genesis state at the end of the code that is displayed.

Now we are ready to set the nodes to mine. 

You can again either run the following command in a new GitBash window, navigating again to the Blockchain-Tools folder or run it in your current GitBash window.

./geth --datadir node1 --unlock "paste address 1 here" --mine --rpc --allow-insecure-unlock

Should look something like this:

./geth --datadir node1 --unlock "0x61AbAa7955ffd11aac8f8dE54F51531E4B42fAfD" --mine --rpc --allow-insecure-unlock

Once this command is executed it will run and ask for the password. Enter the password you created earlier.

Example:

testnet0123

It should then start mining and creating blocks. It will also create an enode. We need to copy this for node2.

It should look similiar to:

enode://2c1d2c8fdab786c72fb1ed3d45e30722c41aa15f7d3a3be4a38eba99bbe32162b0515fbf23b1161cf3b774e3749ae86cae0d5096858756ba82f38d4f3b44a8c0@127.0.0.1:30303

Now for this next part you will need to open a new GitBash window and navigate to the Blockchain-Tools folder again and run the following command:

./geth --datadir node2 --unlock "paste address 2 here" --mine --port 30304 --bootnodes "paste your enode here" --ipcdisable

It should similar to this:

./geth --datadir node2 --unlock "0x3072c6Afd0d847ACF4440E11a2206f8003aaCde9" --mine --port 30304 --bootnodes enode://2c1d2c8fdab786c72fb1ed3d45e30722c41aa15f7d3a3be4a38eba99bbe32162b0515fbf23b1161cf3b774e3749ae86cae0d5096858756ba82f38d4f3b44a8c0@127.0.0.1:30303 --ipcdisable

We set the port to 30304 because the rpc flag on node1 is set to port 8545 and the two nodes cannot be using the same port. Also, without the --ipcdisable at the end the code will show an error access is denied for node2 which will not enable the two nodes to communicate with each other.

## Creating Custom Network & Sending Test Transaction on MyCrypto

Now weare ready to create our own custom network and send a test transaction on MyCrypto.

First, open MyCrypto and on the bottom right, underneath help, in somewhat tiny letters, click on Change Network.

From here at the bottom right, again in somewhat tiny letters we will click Add Custom Node.

First we want to click on the dropdown box underneath Network and change from ETH to Custom, which is at the very bottom of the list. For Node Name and Network Name we are going to enter the name of our netwrok in both spots.

For example:

Auriel

For Currency we are going to select ETH and for ChainID we are going to enter the ChainId we entered when creating our netwrok. 

This is where you can enter a random 3 digit number to ensure it is not taken BEFORE creatign the Chain ID for our network or else, as mentiond previously, you would have to start all over from scratch.

Under URL we will enter http://127.0.0.1:8545

Once all of this information is entered we will click on Save & Use Custom Node

Once our Custom Node has been created we will click on Keystore file. This will open a file explorer type window. We will need to navigate to our Blockchain-Tools folder, go into our node1 folder, and then into our keystore folder and select what should be the only file in this folder and then click open.

We will then need to enter our password we created earlier.

Example:

testnet0123

This should open our first address we created that has been mining and we should a whole lot of "ETH" in our account. Now select Send Ether & Tokens, either fro mthe top or the top drop down box. We can paste our second address into the ToAddress field enter in 1 ETH and click Send Transaction.

If successful a green pop-up box should appear saying, "Your TX has been broadcast to the network...."

And that is how you can create your own private Blockchain Network.

Also, once your done and don't fill like having the program mine anymore you can just exit out of the GitBash windows and they will stop.