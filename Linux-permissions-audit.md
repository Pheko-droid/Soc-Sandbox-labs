markdown
# Linux File Permissions Audit
## Objective:
The objective of this audit was to inspect and updated system file permissions on a Linux server to align with the organization's access control security policies. Proper file permissions ensure that sensitive files (like system logs, user records, and configuration files) can only be written, or executed by authorized personnel.
----
## Scenario Overview:
An internal  security audit revealed that several files in a mock organization's server had loose, none-compliant permissions.
My task was to:
.Navigate to the targeted system directories.
.Check exisiting permissions using the command-line interface (CLI).
.Upadate file permissions and group ownership to match the orgsnization's security baselne (e.g., ensuring sensitive directories are restricted from "Other/Public" users).
----
##Commands and Methodology
##1.Navigating and Listing Permissions
To locate the files and inspect their current permissions, ownership, and group associations, I used the following commands:
'''bash
#Navigate to target directory
cd/home/researcher/projects
#List all files, including hidden ones, with detailed permissions
ls -la
.Command Breakdown:
. ls:Lists directory contents.
.-l:Uses the long listing format(showing permissions,owner,group,file size, and modification date).
.-a:Includes hidden files(files strting with a .).
2.Identifying Permission Discrepeancies
When reviewing the output of ls -la, I analyzed the 10-character permssion string(e.g., -rwxrwxr-x):
. Character1: File type (-for regular file, d for directory).
. Characters2-4:User/Owner permisssions(rwx-Read,Write,Execute).
. Characters5-7:Group permissions(rwx-Read, Write, Execute).
. Characters8-10:Other/Public permissions(rwx-Read, Write, Execute).
I discovered that a sensitive file named project_plans.txt was readable and writable by "Other"(-rw-rw-rw-), violating our security policy.
3.Modifying Permissions(chmod)
To restrict write access for everyone except the file owner,and completely block "Other"users from reading the file,I used the chmod command with absolute(octal)notation:
bash
#Modify permissions of the file
chmod 640 project_plans.txt
. Octal Permission Breakdown(640):
. 6(Owner):Read and Write permissions(4+2=6).
. 4(Group):Read-only permission(4).
. 0(Other):No permission(0).
After running this command, I ran ls -l project_plans.txt to verify that permissions changed from -rw-rw-rw to -rw-r-----.
4.Updating Group Ownership(chown)
To ensure the correvt department had administrative control over the project directory, I updated the group owner to research_team group:
bash
#Change group ownership of the directory
sudo chown :research_team/home/researcher/projects
Verification & Security Posture Improvement
By implementing these changes,the server's security posture was significantly improved:
. Confidentiality:Unauthorzed system users("Other")can no longer read sensitive project plans or system configurations.
. Integrity:Write access is stritctly limited to the file owner, preventing unauthorized modification of research data.
. Audit Compliance: The file system now strictly conforms to organizational access control baselines.
