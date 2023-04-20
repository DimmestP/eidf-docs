# Accessing the Superdome Flex inside the Safe Haven Service

## What is the Superdome Flex?

The [Superdome Flex (SDF)](https://www.hpe.com/psnow/doc/a00026242enw) is a high-performance computing cluster manufactured by Hewlett Packard Enterprise. It has been designed to handle multi-core, high-memory tasks in environments where security is paramount. The hardware specifications of the SDF within the Safe Haven Service (SHS) are:

- 576 physical cores (1152 hyper-threaded cores)
- 18TB of dynamic memory (17 TB available to users)
- 768TB or more of permanent memory

The software specification of the SDF are:

- [Red Hat Enterprise Linux](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/8.7_release_notes/index) (v8.7 as of 27/03/23)
- [Slurm](https://slurm.schedmd.com/quickstart.html) job manager
- Access to local copies of R (CRAN) and python (conda) repositories
- [Singularity](https://docs.sylabs.io/guides/3.5/user-guide/introduction.html) container platform



**Key Point** 

`The SDF is within the Safe Haven Service. Therefore, the same restrictions apply, i.e. the SDF is isolated from the internet (no downloading code from public GitHub repos) and copying/recording/extracting anything on the SDF outside of the SHS is strictly prohibited unless through approved processes.`

## Accessing the SDF

Users can only access the SDF by ssh-ing into it via their VM desktop. 

### Hello world

	**** On the VM desktop terminal ****

	ssh shs-sdf01
	<Enter VM password>

	echo "Hello World"
	
	exit

## SDF vs VM file systems

The SDF file system is separate from the VM file system, which is again separate from the project data space. Files need to be transferred between the three systems in order for any analysis to be completed within the SDF.

### Example showing separate SDF and VM file systems

	**** On the VM desktop terminal ****
	
	cd ~ 
	touch test.txt
	ls

	ssh shs-sdf01
	<Enter VM password>

	ls # test.txt is not here
	exit

	scp test.txt shs-sdf01

	ssh shs-sdf01
	<Enter VM password>
	
	ls # test.txt is here

### Example copying data between project data space and SDF

Transferring and synchronising data sets between the project data space and the SDF is easier with the rsync command (rather than manually checking and copying files/folders with scp). rsync only transfers files that are different between the two targets, more details in its manual.

	**** On the VM desktop terminal ****

	man rsync # check instructions for using rsync

	rsync -avPz -e ssh /safe_data/my_project/current_wip shs-sdf01: # sync project folder and SDF home folder
	
	ssh shs-sdf01
	<Enter VM password>

	*** Conduct analysis on SDF ***

	exit

	rsync -avPz -e ssh /safe_data/my_project/current_wip shs-sdf01: # sync project file and ssh home page # re-syncronise project folder and SDF home folder

	*** Optionally remove the project folder on SDF ***
