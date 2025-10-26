
# Watch Me Build The Lab Here
https://www.loom.com/share/679eeabf425640f886af2365811e678c

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
groupadd Operators
groupadd "Data Analysts"

Verify group creation:

cat /etc/group | grep -E "Developers|Operations|Data Analysts"

### **Step 3: Create User Accounts**
Create the following users and assign them to their respective groups.

| Name | Username | Email | Group |
|------|-----------|--------|--------|
| Jess Waller | jwaller | jwaller@levelupbank.com | Developers |
| Blake Dorsey | bdorsey | bdorsey@levelupbank.com | Operators |
| Joey Ewart | jewart | jewart@levelupbank.com | Data Analysts |

#### Commands:

useradd -m -G Developers -c "Jess Waller, jwaller@linuxtest.com" jwaller
useradd -m -G Operators -c "Blake Dorsey, bdorsey@linuxtest.com" bdorsey
useradd -m -G "Data Analysts" -c "Joey Ewart, jewart@linuxtest.com" jewart

> The `-m` flag creates a home directory for each user.  
> The `-G` flag assigns the user to the specified group.

Set passwords for each user:

passwd jwaller
passwd bdorsey
passwd jewart

### **Step 4: Assign Group Ownership and Permissions**
Now we have created each user, we want to provide each user with Ownership of their assigned Directories

#### Change Group Ownership

chown :jwaller dev_file{1..3}.txt
chown :bdorsey ops_file{1..3}.txt
chown :jewart data_file{1..3}.txt


#### Set Permissions (Read, Write, Execute for Group Only)

chmod 770 /Development
chmod 770 /Operations
chmod 770 /Analytics


> ✅ **Explanation:**
> - `7` gives full permissions (read, write, execute) to the owner and group.
> - `0` denies access to others.


---

## **Conclusion**
This lab successfully demonstrated how to:
- Manage users and groups in Linux
- Apply and verify file permissions
- Enforce least privilege access using `chmod` and `chown`


