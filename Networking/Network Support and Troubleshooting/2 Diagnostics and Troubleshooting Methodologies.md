### Troubleshooting Review
- Identify, locate and correct problems
- Requires gathering info, and using a strategy
- Suggestions for data gathering:
	- Determine the nature of problem
	    - End-user reports
	    - Problem verification report
	- Gather relevant equipment information
	    - Manufacturer
	    - Make / model
	    - Firmware version
	    - Operating system version
	    - Ownership / warranty information
	- Gather configuration and topology information
	    - Physical and logical topology
	    - Configuration files
	    - Log files
	- Determine if there were any similar issues previously
	    - Steps taken
	    - Results achieved

### Seven-step Troubleshooting Process

![[Pasted image 20251112205802.png]]

### Troubleshooting with Layered Models
- Use the OSI model to troubleshoot
- Generally good to start with Layer 3 - can you ping the default gateway for the subnet?
- Routers and multilayer switches operate at L3 but can also operate at L4 when it comes to extended ACLs

![[Pasted image 20251112211056.png]]

### Structured Troubleshooting Methods
- **Bottom-Up** - Start with the physical layer and the physical components of the network and move up through the layers of the OSI model until the cause of the problem is identified.
- **Top-Down** - Start with the end-user applications and move down through the layers of the OSI model until the cause of the problem has been identified.
- **Divide-and-Conquer** - Start by collecting user experiences of the problem, document the symptoms and then, using that information, make an informed guess as to which OSI layer to start your investigation.
- **Follow-the-Path** - Discover the traffic path all the way from source to destination. This approach usually complements one of the other approaches.
- **Substitution** - Physically swap the problematic device or component with a known, working one. If the problem is fixed, then the problem is with the removed item. If the problem remains, then the cause is elsewhere.
- **Comparison** - Compare specifics such as configurations, software versions, hardware, or other device properties, links, or processes between working and nonworking situations and spot significant differences between them.
- **Educated Guess** - A less-structured troubleshooting method that uses an educated guess based on the experience of the technician and their ability to solve problems.

### Guidelines for Selecting a Troubleshooting Method

![[Pasted image 20251112212302.png]]

### Document Findings, Actions, and Outcomes
A technician must document the:
- **Problem** - Includes the initial report of the problem, a description of the symptoms, information gathered and any other information that would help resolve similar problems.
- **Solution** - Includes the steps taken to resolve the problem.
- **Commands and Tools Used** - Include the commands and tools used in diagnosing the problem and solving the problem.
Always verify the solution with the user and have them verify and signal they are satisfied with the result. Then update documentation as necessary