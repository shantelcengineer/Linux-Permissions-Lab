

# AWS Linux Lab: User, Group, and Directory Permissions

## **Objective**
This lab demonstrates how to create users, groups, and directories in an AWS-hosted Linux environment, assign appropriate permissions, and verify user access control. This is a foundational exercise for Linux system administration and IAM management within AWS EC2 instances.

---

## **Environment**
- Platform: AWS EC2 (Amazon Linux 2)
- Privileges: Root user access (`sudo su`)
- Tools: Command-line interface (SSH terminal)


## **Lab Steps**

### **Step 1: Create Directories**
As the root user, create the following directories:

mkdir /Development /Operations /Analytics


#### Add Dummy Files to Each Directory
Use the `touch` command to create several blank files in each directory (no text editor used):

touch /Development/dev_file{1..3}.txt
touch /Operations/ops_file{1..3}.txt
touch /Analytics/data_file{1..3}.txt


### **Step 2: Create Linux Groups**
Create three groups to match your department structure:

groupadd Developers
groupadd Operations
groupadd "Data Analysts"


Verify group creation:

cat /etc/group | grep -E "Developers|Operations|Data Analysts"



### **Step 3: Assign Group Ownership and Permissions**
Each group should have access only to its own directory.

#### Change Group Ownership

chown :Developers /Development
chown :Operations /Operations
chown :"Data Analysts" /Analytics


#### Set Permissions (Read, Write, Execute for Group Only)

chmod 770 /Development
chmod 770 /Operations
chmod 770 /Analytics


> ✅ **Explanation:**
> - `7` gives full permissions (read, write, execute) to the owner and group.
> - `0` denies access to others.


### **Step 4: Create User Accounts**
Create the following users and assign them to their respective groups.

| Name | Username | Email | Group |
|------|-----------|--------|--------|
| Jess Waller | jwaller | jwaller@levelupbank.com | Developers |
| Blake Dorsey | bdorsey | bdorsey@levelupbank.com | Operations |
| Joey Ewart | jewart | jewart@levelupbank.com | Data Analysts |

#### Commands:

useradd -m -G Developers -c "Jess Waller, jwaller@levelupbank.com" jwaller
useradd -m -G Operations -c "Blake Dorsey, bdorsey@levelupbank.com" bdorsey
useradd -m -G "Data Analysts" -c "Joey Ewart, jewart@levelupbank.com" jewart

> The `-m` flag creates a home directory for each user.  
> The `-G` flag assigns the user to the specified group.

Set passwords for each user:

passwd jwaller
passwd bdorsey
passwd jewart


### **Step 5: Verify Directory Access**
Switch to each user and test visibility:

su - jwaller
ls /
ls /Development /Operations /Analytics 2>/dev/null
exit

Repeat for `bdorsey` and `jewart` users.  
Each user should only have access to their group’s directory and files.

---

### **Step 6: Restrict User Profile Visibility**
Ensure that users can only view their own profiles.

chmod 700 /home/jwaller
chmod 700 /home/bdorsey
chmod 700 /home/jewart

This ensures only the profile owner (and root) can view the contents of their home directory.

---

**### **Step 7: Verify User Profile **Permissions**
Switch to each user and attempt to view another user’s directory:

su - jwaller
ls /home/bdorsey
# Should result in "Permission denied"
exit


Repeat this verification for each user.

---

## **Verification Summary**
| Action | Command | Expected Result |
|---------|----------|----------------|
| Group ownership check | `ls -ld /Development /Operations /Analytics` | Correct group ownership assigned |
| Permission check | `ls -l /` | Directories show `drwxrwx---` |
| User access | `su - [username] && ls /` | Only group directory visible |
| Home directory restriction | `ls /home/[other_user]` | Permission denied |

---

## **Conclusion**
This lab successfully demonstrated how to:
- Manage users and groups in Linux
- Apply and verify file permissions
- Enforce least privilege access using `chmod` and `chown`
- Validate compliance with access control principles in an AWS-hosted Linux system

