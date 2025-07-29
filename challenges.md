## **1. Challenge Name:** Updates are TUF\#

   - **Intended Difficulty:** Easy  
   - **Submitter:** Red Balloon Security  
   - **Category:** Exploitation, Reverse Engineering  
   - **Description:** I set up secure software updates using TUF. That way nobody can do a software rollback\! Right?   
     To connect, join the network and run:  

|  | nc 172.28.2.64 8002 |
| :---- | :---: |

   - **Solve Guide:**  
     1. Connect to the device over the network using netcat: \`nc 172.28.2.64 8002\`  
     2. Enter a regular expression that matches \`tcmupdate\_v0.3.0.py\` without using the \`.\` character, for example: \`tcmupdate\_v0\[^a\]3\[^a\]\*\`

---

## **2. Challenge Name:** One Key to Root Them All

   - **Intended Difficulty:** Medium/Hard  
   - **Submitter:** Red Balloon Security  
   - **Category:** Crypto, Exploitation  
   - **Description:** Even if you roll back to an old version, you'll never be able to access the versions I have overwritten\! TUF uses crypto, so it must be super secure. You will need to have solved the previous challenge to progress to this one.   
     To connect, join the network and run:  

|  | nc 172.28.2.64 8002 |
| :---- | :---: |

   - **Solve Guide:**  
     1. Connect to the device over the network using netcat: \`nc 172.28.2.64 8002\`  
     2. Use the exploit from the previous challenge to run \`tcmupdate\_v0.3.0.py\` on the device  
     3. Crack the RSA public key used for the TUF server's targets, snapshot, and timestamp roles (all the same key) using a Fermat factoring attack to recover the private key (for example using \[RsaCtfTool\](https://github.com/RsaCtfTool/RsaCtfTool))  
     4. Use the recovered private key to build and sign new TUF metadata files that enable running an old file that is on the TUF server, but otherwise inaccessible without resigning  
     5. Run your own TUF server (static web host \-- e.g., \`python3 \-m http.server\`) with the updated metadata, and tell the TCM to use your server by typing its address into the update prompt over netcat

---

## **3. Challenge Name:** RPS FTW (Speedometer SAO)

   - **Intended Difficulty:** Medium/Hard  
   - **Submitter:** Uberwoozle  
   - **Category:** Reverse Engineering, Exploitation, Vehicle Network  
   - **Description:** Two cats, one flag (and rock paper scissors)  
     \* The firmware is provided for reversing and figuring out what the behavior is  
   - **Solve Guide (you only need one badge, the boss badge):**  
     1. Connect two SAO in cat mode (you can see them battle and send messages over CAN)  
     2. Send proper speed value (pointer, in speedometer mode) to offset into memory for printing from font library  
     3. Send bad RPS response with offset to speed variable (the value offsets into memory for a pointer to string, ideally the speed value)  
     4. Decode flag embedded in font library from LCD  
        \* Dummy \`flag{REAL\_FLAG\_ON\_THE\_CTF\_BADGE}\`  
        \* You can't read the string of the flag directly, I made bounds check for that, you have to find memory locations for the "ASCII" values 0x79 & 0x80 and throw hand with them which prints the the hexidecimal as "art"

---

## **4. Challenge Name:** CAN you enumerate? (Main CHV Badge)

   - **Intended Difficulty:** Easy  
   - **Submitter:** Linted  
   - **Category:** Vehicle Network  
   - **Description:**   
   - **Solve Guide:**  
     1. Recovered by sending messages to arb 0x6b0

---

## **5. Challenge Name:** Vehicle inspection (Main CHV Badge)

   - **Intended Difficulty:** Easy  
   - **Submitter:** Linted  
   - **Category:** Vehicle Network  
   - **Description:** There is a flag somewhere on the badge… where could it be?  
   - **Solve Guide:**  
     1. Look under the battery pack for the flag

---

## **6. Challenge Name:** Traffic check (Main CHV Badge)

   - **Intended Difficulty:** Easy  
   - **Submitter:** Linted  
   - **Category:** Reverse Engineering  
   - **Description:**  
   - **Solve Guide:**  
     1. Pieced together by random traffic being generated on the badge  
     2. FLAG: if\_the\_washrooms\_are\_clean

---

## **7. Challenge Name:** Test Drive (Main CHV Badge)

   - **Intended Difficulty:** Easy  
   - **Submitter:** Linted  
   - **Category:** Reverse Engineering  
   - **Description:**   
   - **Solve Guide:**  
     1. Printed out when you first connect to the badge's python repl over uart

---

## **8. Challenge Name:** Keep The World Hacking Forever

   - **Intended Difficulty:** Easy/Medium  
   - **Submitter:** Rivian  
   - **Category:** Exploitation  
   - **Description:** Pop a shell on the Rivian HIL  
   - **Tools required:** Ethernet  
   - **Solve Guide:**  
     1. Path traversal from a diagnostics api to recover the SSH key to login to the module.   
     2. Flag in MOTD  
     3. Flag: SSH Key (TBD)

---

## **9. Challenge Name:** Reversing up the mountain

   - **Intended Difficulty:** Hard  
   - **Submitter:** Rivian  
   - **Category:** Reverse Engineering  
   - **Description:** Do Recon on the Rivian HIL to pivot into the vehicle  
   - **Solve Guide:**  
     1. Find an update binary for the CGM on the TCM (you have shell from KTWHF),  
     2. RE the CGM image, find where to grab the flag info from the module over the network.  
     3. Grab the key/data from actual CGM on the HIL (flag) over DOIP, read memory by address  
     4. Flag contains info for getting on truck via Nebula

---

## **10. Challenge Name:** 1337 YEET Adventure

    - **Intended Difficulty:** Hard  
    - **Submitter:** Rivian  
    - **Category:** Vehicle Network  
    - **Description:** Abuse the telematics module to unlock the vehicle  
    - **Solve Guide:**  
      1. With info gleaned from Reversing up the Mountain, you should find the right command to unlock the vehicle over UDS  
      2. User will have to connect to the truck over nebula and figure out how to redirect traffic from telematics to CGM (nebula unsafe routes)  
      3. Unlock the doors, flag inside the vehicle.

---

