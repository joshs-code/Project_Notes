# ACL

### Overview

Access Control Lists (ACLs) in Linux provide fine-grained permission management beyond traditional user, group, and other permissions. They allow you to define permissions for specific users or groups on a file or directory.

### Prerequisites

* Ensure the filesystem supports ACLs (e.g., ext4, xfs).
* Install ACL tools: `sudo apt install acl` (Debian/Ubuntu) or `sudo yum install acl` (RHEL/CentOS).
* Verify ACL support: `mount | grep acl` or check `/etc/fstab` for `acl` mount option.

### Basic Commands

#### 1. View ACLs

*   Display ACLs for a file or directory:

    ```bash
    getfacl <file_or_directory>
    ```

    Example output:

    ```
    # file: file.txt
    # owner: root
    # group: root
    user::rw-
    user:javed:r-x
    group::r--
    mask::r-x
    other::r--
    ```

#### 2. Set ACLs

*   Modify ACLs using `setfacl`:

    ```bash
    setfacl -m <acl_entry> <file_or_directory>
    ```

    * `-m`: Modify ACL.
    * `<acl_entry>` format: `u:<username>:<permissions>` or `g:<groupname>:<permissions>`.
    * Permissions: `r` (read), `w` (write), `x` (execute), or `---` (no permissions).

    Examples:

    *   Grant user `javed` read and execute permissions:

        ```bash
        setfacl -m u:javed:r-x <file_or_directory>
        ```
    *   Grant group `developers` read and write permissions:

        ```bash
        setfacl -m g:developers:rw- <file_or_directory>
        ```

#### 3. Remove ACLs

*   Remove specific ACL entry:

    ```bash
    setfacl -x u:<username> <file_or_directory>
    ```

    Example:

    ```bash
    setfacl -x u:javed <file_or_directory>
    ```
*   Remove all ACLs (restore to standard permissions):

    ```bash
    setfacl -b <file_or_directory>
    ```

#### 4. Set Default ACLs (for directories)

*   Default ACLs apply to new files/directories created within a directory:

    ```bash
    setfacl -m d:u:<username>:<permissions> <directory>
    ```

    Example:

    ```bash
    setfacl -m d:u:javed:rwx /path/to/directory
    ```
*   View default ACLs:

    ```bash
    getfacl <directory>
    ```

    Look for entries starting with `default:`.
*   Remove default ACLs:

    ```bash
    setfacl -k <directory>
    ```

#### 5. Recursive ACLs

*   Apply ACLs to a directory and its contents:

    ```bash
    setfacl -R -m u:<username>:<permissions> <directory>
    ```

    Example:

    ```bash
    setfacl -R -m u:javed:r-x /path/to/directory
    ```

#### 6. Mask

* The `mask` limits the effective permissions of ACL entries (except `owner` and `other`).
*   Set a specific mask:

    ```bash
    setfacl -m m::<permissions> <file_or_directory>
    ```

    Example:

    ```bash
    setfacl -m m::r-x <file_or_directory>
    ```

#### 7. Copy ACLs

*   Copy ACLs from one file to another:

    ```bash
    getfacl <source_file> | setfacl --set-file=- <target_file>
    ```

#### 8. Backup and Restore ACLs

*   Backup ACLs:

    ```bash
    getfacl -R /path/to/directory > acl_backup.txt
    ```
*   Restore ACLs:

    ```bash
    setfacl --restore=acl_backup.txt
    ```

### Common ACL Entry Types

* `user::rwx`: Permissions for the file owner.
* `group::rwx`: Permissions for the file group.
* `other::rwx`: Permissions for others.
* `user:<username>:rwx`: Permissions for a specific user.
* `group:<groupname>:rwx`: Permissions for a specific group.
* `mask::rwx`: Maximum permissions for ACL entries (except owner/other).
* `default:<type>:<name>:rwx`: Default permissions for new files/directories.

### Examples

1.  Grant `rod` read-only access to a file:

    ```bash
    setfacl -m u:rod:r-- /path/to/file
    ```
2.  Deny all permissions to `javed`:

    ```bash
    setfacl -m u:javed:--- /path/to/file
    ```
3.  Set default ACL for a directory so new files grant `javed` read/write:

    ```bash
    setfacl -m d:u:javed:rw- /path/to/directory
    ```
4.  Remove all ACLs from a file:

    ```bash
    setfacl -b /path/to/file
    ```

### Notes

* Use `ls -l` to check if a file has ACLs (look for a `+` after permissions, e.g., `rwxr-xr-x+`).
* ACLs override traditional permissions for users/groups specified in ACL entries.
* Ensure the filesystem is mounted with ACL support (add `acl` to `/etc/fstab` if needed).
* Always test ACL changes in a safe environment to avoid unintended access issues
