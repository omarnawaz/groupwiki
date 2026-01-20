System Access
========================================

Getting access to the HPC computing resources on the Falcon supercomputer requires a few steps. This guide walks you through the process as a new group member.

Important: VPN Requirement
------------------------------------

All access to the Falcon system requires a Virtual Private Network (VPN) connection:


Before proceeding with registration, ensure you have access to the Cardiff University VPN. You can find instructions for setting up the VPN on the `Cardiff University IT Services VPN intranet page <https://intranet.cardiff.ac.uk/staff/supporting-your-work/it-support/wireless-and-remote-access/off-campus-access/virtual-private-network-vpn>`_.

Step 1: Receive Pilot Program Notification
------------------------------------

Falcon is currently in pilot phase. You must receive confirmation that you are enrolled in the pilot program before proceeding with registration.

- Your PI or group leader will notify you once you've been added to the pilot program
- You may also receive notification via the ARCCA Community mailing list
- **Do not proceed to Step 2 until you receive this confirmation**

Step 2: Create Your User Account
------------------------------------

Once you receive pilot program confirmation, you need to register as a Falcon user:

1. Access the user registration portal (your group leader can provide the link)
2. Complete the new user registration form with your Cardiff University details
3. Submit your registration request
4. Wait for confirmation that your account has been created

Step 3: Get Added to the CASA Project
------------------------------------

To access the CASA project resources, you must be added by a project administrator:

1. Notify your group leader that your user account has been created
2. Your group leader (or designated project administrator) will add you to the CASA project
3. Wait for confirmation that you have been enrolled in the project
4. This process typically occurs through the Coldfront system

Step 4: Verify Your Access
------------------------------------

Before attempting to log in, verify that your account is properly configured:

1. Ensure you are connected to the Cardiff University VPN
2. Check that you can access the Coldfront system (project management interface)
3. Confirm your username and project assignment in Coldfront
4. You should now be ready to log in to Falcon

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

Alternative: Using Open OnDemand
------------------------------------

Falcon provides a web-based interface called Open OnDemand that you can use instead of SSH:

1. Ensure you are connected to the VPN
2. Navigate to the Open OnDemand web portal (link provided by your group leader)
3. Log in with your Falcon username and password
4. Access your files, job submissions, and applications through the web interface
5. This can be easier for file transfer and interactive work

File Transfer
------------------------------------

To transfer files to and from Falcon:

1. Use ``scp`` (secure copy) for command-line transfers:

   .. code-block:: console

      scp localfile username@falcon.arcca.cf.ac.uk:/path/to/destination

2. Use ``sftp`` for interactive file transfer:

   .. code-block:: console

      sftp username@falcon.arcca.cf.ac.uk

3. The Open OnDemand interface also provides a graphical file manager

Troubleshooting
------------------------------------

**Cannot connect to Falcon:**
- Verify you are connected to the Cardiff University VPN
- Check that your username and password are correct
- Ensure your account has been added to the CASA project

**Permission denied errors:**
- Confirm you can access Coldfront successfully
- Verify your project assignment in Coldfront
- Contact your group leader or ARCCA support

**Lost access after registration:**
- Your account may have been removed if you are no longer part of the pilot program
- Contact ARCCA support or your group leader to reinstate access

Need Help?
------------------------------------

If you encounter issues during the registration or access process:

- Contact your group leader first—they can often resolve issues directly
- Reach out to ARCCA support for technical issues
- Check the `Falcon Documentation <https://wiki.arcca.cf.ac.uk/index.php/Falcon>`_ for more detailed information

Remember: You must be on the VPN to access any Falcon services, regardless of whether you are on campus or not.
