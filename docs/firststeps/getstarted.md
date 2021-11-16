---
hide:
  - navigation
---

# Get Started with LUMI

[terms-of-use]: https://www.lumi-supercomputer.eu/lumi-general-terms-of-use_1-0/
[support-account]: https://lumi-supercomputer.eu/user-support/need-help/account/
[myaccessid-profile]: https://mms.myaccessid.org/profile/
[puttygen]: https://www.puttygen.com/#How_to_use_PuTTYgen
[support]: https://lumi-supercomputer.eu/user-support/need-help/
[registration]: ../accounts/registration.md
[sample-ssh-keygen-output]: sample-ssh-keygen-output.md
[note-on-ssh-passphrase]: #note-on-ssh-passphrase
[bad-complex-password]: https://xkcd.com/936/
[correcthorsebatterystaple]: https://correcthorsebatterystaple.com/
[torproject]: https://torproject.org/

Please read through all of this carefully before you start running on LUMI. Here
we describe a few sets of basic rules and the important information that you
need to get started.

The process of obtaining access to LUMI consists of two main steps.

 - Registering for your person an identity within the PUHURI system

 - Setting up an SSH key pair

## Registering your identity
First your person has to be identified within PUHURI. This process is generally initiated by national LUMI allocator creating a project within PUHURI, allocating and approving resources for it and creating an invitation to you email address as it is known to the resource allocator. PUHURI then sends an invitation link to your email address.

Using your browser, you access this invitation link several times for several registration steps (accepting invitation, creating MyAccessId account, accepting terms and conditions and privacy) until your personal identity builds up required properties, at which point CSC.FI creates a corresponding project and your new LUMI username and sends you email notifying you. Within about an hour or two after receiving this email, you should be able to SSH in into a LUMI login node. See section "How to log in" below.

Please note that you will not be asked for or provided with LUMI account password. LUMI access does not use password authenticaion. It uses authentication via SSH *public* key only. Its corresponding *private* key should be protected by a passphrase (explained below) which you and only you should know, as it becomes the gateway to your LUMI access authorization.

As you create MyAccessId account, you are required to upload your SSH public key. If you are unsure what it is or how it works, see the "Setting up SSH key pair" section below.


### Summary of identity registration steps:

1) Access the Invitation Link and follow instructions.

2) Accept the Documents presented in MyAccessId

3) Access the Invitation Link to create the account

4) Upload SSH your public key. If you don't have one yet, see the "Generate your SSH keys" section below.

LUMI Countries have different kinds of portals managing user access to the system. Please contact your local HPC organization to find which URL to go to. The portals will lead you to MyAccessID registration age, where you have to accept the Acceptable Use Policy and LUMI Terms of Use 
document, which is linked there. Please read it carefully! 

<figure>
  <img 
    src="../../assets/images/Puhuri_Registration_example.png" 
    width="480"
    alt="Screenshot of registration portal"
  >
  <figcaption>MyAccessID Registration portal</figcaption>
</figure>

 
You may also modify the email address, but according to 
[LUMI Terms of Use][terms-of-use] you must use your organizational email address.

The authentication in the portal is done with home organization identity provider,
which can be selected from the list. In case that is not possible please 
[contact the support team][support-account] with the error message, and you may also
contact your identity provider directly.

You also need to be a member of a project. The project's PI will create a project 
and invite members based on email addresses. Resource allocators of each country will 
accept the project. When the project is accepted, the user accounts will be 
created in LUMI. You will receive email from CSC's Identity management system 
informing you of your project ID and user account name.


## Setting up SSH key pair

**You can only log in to LUMI using SSH keys**. There are no passwords in the LUMI system. In order for this to work, you need to register your SSH *public* key with MyAccessID, from where LUMI will fetch it.


### Generate your SSH keys

After registration, you need to register a **public** key (**Note! The SSH key pair must be a 4096-bit RSA or elliptic curve type key pair**). In order to do that you need to generate an SSH key pair. How to generate 4096-bit RSA key pair:

=== "From a terminal (all OS)"

    This section is intended for users that are not familiar with details of SSH
    usage. If you already know how SSH works, feel free to modify these instructions,
    at your own risk and peril, of course.
    
    An SSH key pair can be generated in the Linux, macOS, Windows PowerShell and 
    MobaXterm terminal. It is important to create a long enough key length. For
    example, you can use the following command to generate a 4096 bits RSA key
    and display your public key which you will later need to paste into MyAccountId.org :

    ```bash
     (
    mkdir $HOME/.ssh/ && chmod 700 $HOME/.ssh
    ssh-keygen -t rsa -b 4096  -f $HOME/.ssh/id_rsa_lumi
    ls -l  $HOME/.ssh/id_rsa_lumi $HOME/.ssh/id_rsa_lumi.pub
    echo =+=+=+=+=+=+=+= Your public key is [algorithm, key, comment] :
    cat  $HOME/.ssh/id_rsa_lumi.pub
    echo =+=+=+=+=+=+=+=
      )
    ```
    
    After running the above, your terminal should look similar to
    this [keygen output][sample-ssh-keygen-output].
   
    During the execution of the above command, you will be asked for a passphrase.
    See [important note][note-on-ssh-passphrase] about the passhrase.

    After that a SSH key pair is created. You should have files named
    `id_rsa_lumi` and `id_rsa_lumi.pub` in your `$HOME/.ssh` directory.

=== "With MobaXTerm or PuTTY (Windows)"

    An SSH key pair can be generated with the PuTTygen tool or with MobaXterm 
    (**Tools :octicons-arrow-right-16: MobaKeyGen**). Both tools are identical.
    
    In order to generate your key pairs for LUMI, choose the option RSA and
    set the number of bits to 4096. The, press the *Generate* button.

    <figure>
      <img src="../../assets/images/win-keygen-step1.png" width="400" alt="Create SSH key pair with windows - step 1">
    </figure>

    You will be requested to move the mouse in the Key area to generate some 
    entropy; do so until the green bar is completely filled.

    <figure>
      <img src="../../assets/images/win-keygen-step2.png" width="400" alt="Create SSH key pair with windows - step 2">
    </figure>

    After that, enter a comment in the Key comment field and a strong
    passphrase. See [important note][note-on-ssh-passphrase] about the passhrase.

    <figure>
      <img src="../../assets/images/win-keygen-step3.png" width="400" alt="Create SSH key pair with windows - step 3">
    </figure>

    The next step is to save your public and private key. Click on the *Save 
    public key* button and save it to the desired location (for example, with 
    `id_rsa_lumi.pub` as a name). Do the same with your private key by clicking
    on the *Save private key* button and save it to the desired location (for 
    example, with `id_rsa_lumi` as a name).

### Note on SSH passphrase

    Please read carefuly the following note
    
!!! warning "Note"
    Please choose a secure passphrase. Keep in mind that a good passphrase does not have
    to be a hard one to remember.
    
    Those of you who fancy an occasional computability calculation can appreciate
    [this][bad-complex-password] explanation, while others can take our word for it.
    
    You may prefer not to use contrapted passwords that are difficult to remember.
    Make sure that the passphrase is at least 20 characters long and easy to remember.
    
    Example of a good passphrase generator is [here][correcthorsebatterystaple].

    Note for the security extra-cautious people: Any online website that sends you a password
    (like the [correcthorsebatterystaple.com][correcthorsebatterystaple] generator), or
    receives from you a password, should be accessed in google-agnostic and facebook-agnostic
    and other-privacy-endangering-giant-agnostic ways, such as with cleared
    cookies in your browser or better yet, clear of all traceable information (including IP address),
    like using the [Tor browser][torproject]. Otherwise you are risking that these nonprivacy
    giants will on top of your usernames also know your where and when you generated passwords
    (and therefore possibly which) to various online services or offline files,
    like your new private SSH key.
    
    **Do not leave the passphrase empty**.
    
    **Write down this passphrase to a secure place.**
    
    **This passphrase will be needed later to log in.**
    
    **Do not give this passphrase to anyone.**
    
    **Do not include this passphrase, or the private key, in any support requests.**
    (There is no legitimate use of this passphrase by any support personnel, at any time.
    Report anyone asking you to give them your passphrase or password, immediately, please.)

    The SSH private key, with or without passphrase, should never be shared with anyone,
    not even with LUMI staff. It should also be stored only in the local computer (public key
    can be safely transmitted to any public place). Protect the private key with a good passphrase!
    Otherwise, anyone with access to the file system can steal your SSH key.

### Upload your public key 
 
Now that you have generated your key pair, you need to upload your SSH **public** key
into your [user profile][myaccessid-profile]. From there, the SSH public key will be 
copied to LUMI with some delay according to the synchronization schedule. This schedule
is currently at once every 10 minutes.

To register your key, click on the *Settings* item of the menu on the left
as shown in the figure below. Then select *Ssh keys*. From here you can add a new public key
or remove an old one. **Note:** SSH key structure is *algorithm, key, comment*.

<figure>
  <img 
    src="../../assets/images/MyAccessID_ssh-key.png" 
    width="480"
    alt="Screenshot of user profile settings to setup ssh public key"
  >
  <figcaption>MyAccessID Own profile information to add ssh public key.</figcaption>
</figure>

After registering the key, there can be a about 15 minutes delay until it is synchronized.
After that your new SSH key should be recognized and accepted by LUMI login nodes.

## How to log in

Connect using a ssh client:

```bash
ssh  -i $HOME/.ssh/id_rsa_lumi  _your_lumi_username_@lumi.csc.fi
```

where you need to replace `_your_lumi_username_` with your own username, which you received
via email during the registration. If you cannot get a connection at all, your 
IP number range might be blocked from login. Please contact the
[support][support-account]. If you can get an SSH connection but LUMI is not logging
you in and is not giving you command prompt, please contact the [support][support-account],
pasting into the support request text the textual screen scrape of your terminal with the
outputs of the following commands:

```bash
 (
ssh-keygen -y -f $HOME/.ssh/id_rsa_lumi
ssh -vvv -i $HOME/.ssh/id_rsa_lumi  _your_lumi_username_@lumi.csc.fi
 )
```

## Running

When you log in to LUMI, you end up on one of the login nodes. These login nodes
are shared by all users and they are not intended for heavy computing.

The login nodes should be used only for:

- compiling (but consider allocating a compute for large build jobs)
- managing batch jobs
- moving data
- light pre- and postprocessing (a few cores / a few GB of memory)

All the other tasks should be done on the compute nodes either as normal batch
jobs or as interactive batch jobs. Programs not adhering to these rules will be
terminated without warning.

Compute intensive jobs must be submitted to the job scheduling system. LUMI uses
Slurm as the job scheduler. In order to run, you need a project allocation. 
You need to specify your project ID in your job script (or via the command line
when submitting your job) in order for your job to be submitted to the queue. 

!!! missing

    Commands to gather information about the project and quota are not
    available yet. However, you can use the `groups` command to retrieve your 
    project ID when connected to LUMI: you should see that you are part of a 
    group named `project_xxxxxxxxx`.

Here is a typical batch script for Slurm. This script runs an application
on 2 compute nodes with 16 MPI ranks on each node (32 total) and 8 OpenMP 
threads per rank.

```
$ cat batch_script.slurm
#!/bin/bash -l
#SBATCH --job-name=test-job
#SBATCH --account=<project_xxxxxxxxx>
#SBATCH --time=01:00:00
#SBATCH --nodes=2
#SBATCH --ntasks=32
#SBATCH --ntasks-per-node=16
#SBATCH --cpus-per-task=8
#SBATCH --partition=standard

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
srun ./application
```

- [More information about running jobs on LUMI](../computing/index.md)

## Where to store data

On LUMI, there are several disk areas: home, projects, scratch (LUMI-P) and fast 
flash-backed scratch (LUMI-F). Please familiarize yourself with the areas and 
their specific purposes.

|              | Path                       | Description                                                                            |
|--------------|----------------------------|----------------------------------------------------------------------------------------|
| **Home**     | `/users/<username>`        | for user configuration files and source code                                           | 
| **Project**: | `/projappl/<project_name>` | act as the project home directory                                                      |
| **Scratch**  | `/scratch/<project_name>`  | intended as temporary storage for input, output or checkpoint data of your application |

- [Learn more about the LUMI storage](../storage/index.md)

## Compiling and Developing your Code

LUMI comes with multiple programming environments: Cray, GNU, and AOCC. 
In addition, the most common libraries used in an HPC environment tuned for LUMI
are also available. Parallel debugger and profiling tools are also at one's 
disposal.

- [Learn more about the programming environments](../development/compiling/prgenv.md)
- [Learn more about debugging](../development/debugging/gdb4hpc.md)
- [Learn more about profiling](../development/profiling/index.md)

## Getting Help

The LUMI User Support Team is here to help if you have any questions or problems
regarding your usage of LUMI. You can contact the support team [here][support].
