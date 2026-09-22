# FileRise move and delete treat a directory as a file

https://github.com/error311/FileRise/security/advisories/GHSA-ch45-p5v4-79c3

Severity: high (CVSS 8.3, `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:L`). CWE-863, with CWE-20. The advisory record also lists CWE-843. No CVE.

FileRise 3.7.0 up to but not including 3.25.0 is affected. Fixed in 3.25.0. The version field stored on the advisory still says 3.24.0 and earlier. The text we agreed uses 3.7.0 as the earliest confirmed release.

`moveFiles` and `deleteFiles` take a bare name and handle it as a file. They never check whether that name is a directory, so a logged-in user can move or delete directories their folder ACL does not cover. A move takes the directory out of the tree the caller was allowed to see.

WebDAV is not in this bug. It already tells files and directories apart and refuses the directory operation. The other file-level endpoints that take a bare name were not affected either. `copyFiles` still has no explicit type check after the fix. Passing it a directory fails on both 3.24.0 and 3.25.0, so it is safe for a different reason. I told the vendor. It is not part of this advisory.

I first scored it 8.8 with availability high. An administrator can restore from trash, and I did not show a permanent delete, so availability is low. 8.3 is the published score.

I retested 3.25.0. Directory move and delete are refused. Moving and deleting a normal file still works.

Same CVE arrangement as the rest of this review. The vendor was going to request it. I did not send a parallel request. As of 22 September 2026 there is no CVE id.

Local Docker only, `error311/filerise-docker` at v3.24.0 (commit 765eccc) and v3.25.0.

Reported privately on 31 July 2026. The vendor published the advisory on 12 August 2026.
