These are the details for the MCP faults.

The faults in EID ACI fabric are 2 types:
1-	Number of VLANs operational per interface exceeded 256 and the switches configured are:
Switch ID	Port channel number
221	Po2, Po3
222	Po2, Po4
311	Po15, Po17
312	Po16, Po20
407	Po5, Po8
408	Po17 , Po30
415	Po12 , Po8
416	Po10, Po16
803	Po1 , Po2
804	Po1 , Po2
So as a result , MCP is only enabled for the first 256 VLANs per interface. 
Impact here, it can cause loops on those vlans/interface not enabled for MCP.
 
2-	Number of logical ports (port x VLAN) which operational per leaf switch exceeds 2000. These are the switches ID’s:
(107 , 108 , 109 , 110 ,113 , 114,115 , 116, 119 ,120 , 127,128 ,129 ,130 ,133 ,134, 139,140 ,209 , 210 ,331 , 332,535,536,605,606,615,616,701,702,703,704,,705,706,707,708,709,,710,711,,712,713,714,717,718,,719,720,721,722,811,812)
 
               Impact here , it could cause the switch to reload.               
 
Additionally , as a guideline for new requests , we sent these below instructions to Ops colleagues who handle ACI changes:
 
1- check first total number of VLANs on the switch based on ethernet interfaces. Exclude from the output number of VLANs on Port-channels
#show mcp internal info interface all | egrep -i 'Interface: Ethernet1|Interface: Po|Number of active VLANs' | egrep -A1 "Ethernet" | egrep "VLANs" | awk '{print $5}' | paste -sd+ | bc
 
if the current number is equal or higher than 1990 , stop the change.
if the current number is lower than 1990 and the number after the change is going to be higher than 1990 , stop the change.
Otherwise , proceed.
 
2- Check total number of active VLANs on the interface:
#show mcp internal info interface <interface>  | grep 'Number of active VLANs'
 
if number is equal or higher than 250 , stop the change. 
if number is lower than 250 and the number after the change is going to be higher than 250 , stop the change.
Otherwise , proceed
 
