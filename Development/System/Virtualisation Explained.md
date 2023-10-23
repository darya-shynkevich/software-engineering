Virtualisation splits up a computer's physical components into multiple virtual computers, promoting isolation and abstraction. It allows businesses to run multiple applications in a single server, making more efficient use of hardware.
![[Pasted image 20230605113054.png]]
Virtual machine is a software based computer. When reasoning about virtual machines its important to understand the two main components:

1. ***GuestOS***. *It's whatever operating system you are trying to run*
2. ***Hypervisor*** *are software that manages and coordinates virtual machines running on physical hardware. It manages everything from allocating resources to virtual machine isolation. Ensuring all machines have the required resources and ensuring issues with one virtual machine doesn't impact the other.* 
	   - ***Type 1 or Bare Metal Hypervisors (OpenKVM)*** *run directly on physical hardware, reducing latency and improving overall efficiency.*
	   - ***Type 2 or Host Hypervisors (VirtualBox)*** *which run as an application on top of another host operating system and often incur a performance hit due to the extra abstraction layer.*

	*Since there is a clean separation between processes on the hostOS and the guestOS, the isolation level is superior. The trade-off here is performance and efficiency, but a big step up from running a single process per machine.* 

References:
1. https://architecturenotes.co/virtualization-explained/ 