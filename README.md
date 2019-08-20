# Verify the format of the Junos commit comment

[Verify_commit_comment.slax](https://github.com/jumation/verify-commit-comment/blob/master/verify_commit_comment.slax) is a tiny Junos commit script which checks that the format of the commit comment matches the company policy.

## Overview

Let's say, that a company policy requires Junos commit comment to follow a strict format of
```
<comment> / <department_code> / <change_type_code> / <change_id>
```

Example of a valid commit comment:

![commit comment screenshot](https://github.com/jumation/verify-commit-comment/blob/master/valid_commit_example.png)

* `<comment>` is a minimum 4 and maximum 140 characters long meaningful message describing the change.

* `<department_code>` is a company-internal two-letter code for a department which initiated the change in the network change management system. For example, `GN`(global NOC) or `UE`(US engineering).

* `<change_type_code>` is a three-letter code for change type in network change management system. For example, `SCH`(standard change), `ECH`(emergency change), `SUP`(software upgrade), etc.

* `<change_id>` is an eight-digit change ID in network change management system. For example, `00475822`.

Those four fields, along with the timestamp, author and method(for example, `cli`, `netconf`) of the configuration commit can then be stored in remote configuration version-control system.

*Screencast of verify_commit_comment.slax:*

![commit comment screencast](https://github.com/jumation/verify-commit-comment/blob/master/screencapture_verify_commit_comment.gif)

As seen above, commit succeeds after the administrator has fixed his typo in department code.


## Installation

Copy(for example, using [scp](https://en.wikipedia.org/wiki/Secure_copy)) the [verify_commit_comment.slax](https://github.com/jumation/verify-commit-comment/blob/master/verify_commit_comment.slax) to `/var/db/scripts/commit/` directory and enable the script file under `[edit system scripts commit]`:

```
martin@vmx1> file list detail /var/db/scripts/commit/verify_commit_comment.slax     
-rw-r--r--  1 martin wheel      1680 Nov 7  21:43 /var/db/scripts/commit/verify_commit_comment.slax
total files: 1

martin@vmx1> show configuration system scripts | display inheritance no-comments    
commit {
    file verify_commit_comment.slax {
        checksum sha-256 b0d0d9a1d49c9d7142f65aba249c551e756aaf41fd020468eae20c68c345bb3c;
    }
}
synchronize;

martin@vmx1> 
```

In case of two routing engines, the script needs to be copied to the `/var/db/scripts/commit/` directory on both routing engines.


## License

[GNU General Public License v3.0](https://github.com/jumation/verify-commit-comment/blob/master/LICENSE)
