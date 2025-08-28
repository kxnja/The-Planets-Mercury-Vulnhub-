# The Planets : Mercury (Vulnhub)
## Setup

1. Install VirtualBox (https://www.virtualbox.org/) and the kali Linux .ova file for VirtualBox (https://www.kali.org/get-kali/#kali-virtual-machines).
2. Install the Mercury.ova file from vulnhub (https://download.vulnhub.com/theplanets/Mercury.ova).
3. Run the VirtualBox file first to set up the virtual machine and do not change any other settings.
4. Run the kali Linux.ova file.
5. Run the mercury.ova (Both the mercury an kali files should pen on virtual box).
6. Right click on the kali Linux machine (DO NOT POWER IT ON FIRST) then open settings and go to the network setting and change it to the internal network from NAT (CHANGE THE NETWORK NAME TO INTNET to avoid difficulties later, I'll explain how).Do the same for the mercury application. (Change it from bridged adapter to internal network and MUST have the same network name as the kali machine that is INTNET)This is just to make sure they are on the same network.
7. Set up a DHCP server - This is essential to assign IP addresses to your devices (your kali linux machine and mercury)
	a. Open command prompt by typing cmd on the search box on the task bar (on windows though it is fairly similar on mac or linux)
    b. Type in `cd /Program Files/Oracle/VirtualBox`
   and click enter (I recommend copying and pasting to avoid errors)
    c. type in   `vboxmanage dhcpserver add --network=intnet --server-ip=10.38.1.1 --lower-ip=10.38.1.110 --upper-ip=10.38.1.120 --netmask=255.255.255.0  --enable`      (I'll explain ,I know it's a lot)
  8. Final step is now going back to start your kali and mercury VM (cheers and happy hacking ethically of course )

# Explanation

So essentially what we have done is that we have used the vboxmanage tool to create a dhcp server for our network (that is why I suggested we use the same name to avoid misconfiguring ) we gave the server the IP - 10.38.1.1 and gave it a range of 10.38.1.110 to 10.38.1.120 to assign to connected devices.
    The netmask basically only defines what part is the network and what part is the host (you). We then finish by enabling it . 

# Note

  Also in this procedure the VMs are completely isolated from your PC and hence you might not be able to browse on your kali VM.
  We isolate it because mercury in this case being a vulnerable machine hackers might exploit it and get into your system.
  
 IF YOU HAD ALREADY DONE THIS FROM THE MR ROBOT CTF (https://github.com/kxnja/MrRobot-CTF-Writeup/tree/main) THERE IS NO NEED TO DO IT AGAIN THE ONLY THING NEEDED IS CHANGING THE NETWORK SETTINGS TO INTERNAL NETWORK AND EVERYTHING ELSE WILL WORK.
