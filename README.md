# IoT-Blink
Simple Blink Example using Raspberry Pi, LED Light and Ethereum for Meetup Class

All sources:
 - https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-ARM
 - http://thejackalofjavascript.com/getting-started-raspberry-pi-node-js/
 - http://thejackalofjavascript.com/raspberry-pi-node-js-led-emit-morse-code/
 - https://www.raspberrypi.org/forums/viewtopic.php?f=66&t=127939
 - https://github.com/ethereum/mist/releases



This assumes you have a Raspberry Pi and can terminal into it.

1.  Install Geth - https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-ARM

        1.1 Get file: https://build.ethdev.com/builds/ARM%20Go%20master%20branch/geth-ARM-latest.tar.bz2 
            (If needed, transfer file to Pi):
             from mac: 'scp ~/Downloads/geth-ARM-latest.tar.bz2 pi@192.168.1.15:~'
            (enter password to pi to complete transfer of file).

        1.2 ssh back into Pi: ssh pi@192.168.1.15 <- your Pi ip address here 
            then untar (decompress) file
            'cd ~'
            'tar -vxjf geth-ARM-latest.tar.bz2'
             then run from there or follow the bin installation step in the original link, and then run
            './geth --fast console'
             (‘—fast’ option for quick sync… otherwise this will take around 10 days on an average Pi vs. 1 day).
     
2.  Install NodeJS
    source:  http://thejackalofjavascript.com/getting-started-raspberry-pi-node-js/
    notes: can use a 2nd terminal window and get all the way to testing an LED with nodejs, but without ethereum, while the geth node is     syncing. 

        2.1 (first update)
        'sudo apt-get update -y && sudo apt-get upgrade -y'

        2.2 Check to see if your pi already has node with:
        'node -v' 
        If not, then download latest node version for arm:
        'wget http://node-arm.herokuapp.com/node_latest_armhf.deb'
        install that bad boy
        'sudo dpkg -i node_latest_armhf.deb'
        test run of node - calls for version, if version present, then you're ready to move on.
        'node -v'


3. Install npm on RPi 
   
  'sudo apt-get install npm -y'

4. Create node project folder:

  'mkdir blink'

  'cd blink'

  'npm init'
    

5. In that project folder, install onoff and web3 modules

        5.1 Install onoff
        'npm install onoff --save'
    
        I needed to apply fix to current release of Jessie Raspbian from errors from this command
        source: https://www.raspberrypi.org/forums/viewtopic.php?f=66&t=127939

        edit file:
        'sudo nano /usr/include/nodejs/deps/v8/include/v8.h'
        in that file change:
        enum WriteOptions {
            NO_OPTIONS = 0,
            HINT_MANY_WRITES_EXPECTED = 1,
           NO_NULL_TERMINATION = 2,
           PRESERVE_ASCII_NULL = 4,
         };

        to this:
        enum WriteOptions {
           NO_OPTIONS = 0,
           HINT_MANY_WRITES_EXPECTED = 1,
           NO_NULL_TERMINATION = 2,
           PRESERVE_ASCII_NULL = 4,
           REPLACE_INVALID_UTF8 = 0
        };
 
        5.2 Install web3
        'npm install web3 --save'
    

6.  Wire up your LED and let's make sure your LED can be blinked by a nodejs program by following these directions:
http://thejackalofjavascript.com/raspberry-pi-node-js-led-emit-morse-code/


7. Let's use the blockchain now. 

        7.1 Assuming the geth is completed syncing lets stop the process and restart with rpc.
            './geth --rpc console'
    
        7.2 Run ethtest.js in a separate terminal instance (put that in your blink dir)
            'sudo node ethtest.js'
            needs sudo to control GPIO
            
    
8. Load contract in your ethereum-wallet on local computer

        8.1 Use this: https://github.com/ethereum/mist/releases  
        
        8.2 On contract tab, load contract from ethtest.js.  
              Using the existing contract this address will be: 0x9535eb707582edb3317dfdcdb751ce41865005fc 
              If you deployed your own contract, then use that address.
              
        8.3 Interact with the contract through the wallet by using set function in bottom right of window and set an integer value. 
        
        8.4 sign the transaction (currently costs around 0.0012 ether) and send it off.

        8.5 2 blocks later, you should have a blinking led assuming step 6 worked for you.  Console window of nodejs ethtest.js should also output a blink message.                 


   







