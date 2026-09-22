# FileRise: move and delete treat directories as files

https://github.com/error311/FileRise/security/advisories/GHSA-ch45-p5v4-79c3

Affects FileRise 3.7.0 up to, but not including, 3.25.0. Fixed in 3.25.0.
The version field stored on the advisory still says `<= 3.24.0`. The text we agreed is 3.7.0 as the earliest confirmed release.

High. CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:L (8.3).
CWE-863, CWE-20. The advisory record also lists CWE-843.
No CVE assigned.

`moveFiles` and `deleteFiles` take a bare name and handle it as a file. They never check whether that name is a directory, so a logged-in user can move or delete directories outside the folders their ACL allows. WebDAV is not in this bug. It already tells files and directories apart and refuses the directory operation.

I first called availability High and scored it 8.8. An administrator can restore from trash, and I did not demonstrate permanent deletion, so availability is Low. 8.3 is the score on the published advisory.

Reported 2026-07-31 through GitHub private vulnerability reporting. The vendor published the advisory on 2026-08-12.

L0stHeart
