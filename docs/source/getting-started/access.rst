System Access
========================================

Getting access to the HPC computing resources on the Falcon supercomputer requires a few steps. This guide walks you through the process as a new group member. Much of this is summarized from the official ARCCA documentation, but tailored for CASA group members. The official documentation is available here: https://wiki.arcca.cf.ac.uk/index.php/New_User_Falcon_Registration

Important: VPN Requirement
------------------------------------

All access to the Falcon system requires a Virtual Pgrivate Network (VPN) connection:


Before proceeding with registration, ensure you have access to the Cardiff University VPN. You can find instructions for setting up the VPN on the `Cardiff University IT Services VPN intranet page <https://intranet.cardiff.ac.uk/staff/supporting-your-work/it-support/wireless-and-remote-access/off-campus-access/virtual-private-network-vpn>`_.

Step 1: Receive Pilot Program Notification
------------------------------------

Falcon is currently in pilot phase. You must receive confirmation that you are enrolled in the pilot program before proceeding with registration.

- Send Omar an email indicating that you have installed the VPN and are ready to join the pilot program.
- Omar will confirm your enrollment in the pilot program via email.
- **Do not proceed to Step 2 until you receive this confirmation**

Step 2: Create Your User Account
------------------------------------

Once you receive pilot program confirmation, you need to register as a Falcon user:

1. Go to https://cogs.cf.ac.uk/ and login with your Cardiff University credentials
2. You will be prompted to enter your information and brief detail about why you need an account.
3. This prompts ARCCA to create an account for you. This can take up to 24 hours.
4. After you get an approval email, log back into COGS and click on the reset SCW Falcon password link to set your Falcon password.
5. Use your new Falcon username and password to log into Coldfront (https://coldfront.arcca.cf.ac.uk/)
6. Email Omar your exact username and he will add you to the relevant project.
7. You should now be able to access Falcon. Proceed to the next step


Logging In to Falcon
------------------------------------

Once your access is verified, you can connect to the system:

1. **Ensure you are connected to the VPN** (this cannot be stressed enough!)
2. Open your terminal and use SSH to connect:

   .. code-block:: console

      ssh username@falcon.arcca.cf.ac.uk

3. Replace ``username`` with your Falcon username
4. Enter your password when prompted
5. You should now have access to the Falcon system

Troubleshooting
------------------------------------

**Cannot connect to Falcon:**
- Verify you are connected to the Cardiff University VPN
- Check that your username and password are correct
- Ensure your account has been added to the CASA project

**Permission denied errors:**
- Confirm you can access Coldfront successfully
- Verify your project assignment in Coldfront
- Contact Omar first, he will follow up with ARCCA support if needed
