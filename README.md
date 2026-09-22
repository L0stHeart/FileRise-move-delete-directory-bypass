# FileRise: move and delete treat directories as files

https://github.com/error311/FileRise/security/advisories/GHSA-ch45-p5v4-79c3

FileRise is a self-hosted PHP file manager (error311/FileRise). I reviewed v3.24.0, commit `765eccc`, and reported this on 2026-07-31. The vendor published the advisory on 2026-08-12. Fixed in 3.25.0. No CVE has been assigned.

`moveFiles` and `deleteFiles` take a bare name and operate on it as a file. They do not check whether the name is a directory. A logged-in user can move or delete directories that their folder ACL does not cover. Confidentiality goes with the move, because the directory leaves the tree the caller was allowed to see. Integrity and availability go with both operations.

WebDAV is not affected. It is the one entry point that already distinguishes files from directories and refuses the directory operation. I also checked the other file-level endpoints that take a bare name. They were not affected. `copyFiles` did not get an explicit type check in the fix either. Passing it a directory fails on both 3.24.0 and 3.25.0, so it is safe for a different reason. I told the vendor that. It is not part of this advisory.

I scored the original report 8.8, with availability High. That overstated it. An administrator can restore from trash, and I did not show a permanent delete. Availability is Low. The published score is 8.3. Primary weakness is CWE-863. I also agree with CWE-20 as the secondary. The advisory record still lists CWE-843 alongside CWE-863.

The version field on the advisory object says `<= 3.24.0`. The text we agreed is narrower on the bottom and the same on the top: 3.7.0 is the earliest release the vendor would confirm from repository history.

| | |
|---|---|
| Affected | 3.7.0 ≤ version < 3.25.0 |
| Fixed | 3.25.0 |
| Severity | High, 8.3 |
| Vector | CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:L |
| CWE | CWE-863, CWE-20. Record also has CWE-843. |
| Privileges | Any logged-in user |
| CVE | Not assigned |

I retested 3.25.0. The directory move and delete are refused. Normal file move and delete still work.

Same CVE arrangement as the rest of this review. The vendor was going to request it. I did not send a parallel request. As of 2026-09-22 there is no CVE id.

Local Docker only, `error311/filerise-docker` at v3.24.0 and v3.25.0.

L0stHeart
https://github.com/L0stHeart
