## Vertical privilege escalation

Access control defines which users are allowed to access specific resources or perform certain actions in an application, based on their identity and permissions. For example, if a regular user can access an admin page and delete accounts by manually visiting an admin URL, this is a broken access control issue known as vertical privilege escalation.

## Unprotected functionality

Unprotected functionality happens when sensitive features are not properly restricted, allowing users to access them just by knowing or guessing the URL. For example, a normal user could access `https://insecure-website.com/admin` or discover a hidden admin URL inside JavaScript code, even though they are not an administrator (for example : admin-325325 which you can find by inspecting the javascript)

## Parameter-based access control methods

Parameter-based access control occurs when an application relies on user-controlled values like cookies or URL parameters to decide permissions.

- A hidden field.
- A cookie.
- A preset query string parameter.

Access control can be insecure when it relies on user-controlled elements such as a hidden field (e.g. `<input type="hidden" name="isAdmin" value="false">` changed to `true`), a cookie (e.g. `Admin=false` edited to `Admin=true`), or a query string parameter (e.g. `dashboard?admin=false` modified to `admin=true`). In each case, an attacker can simply change the value to gain unauthorized access to restricted features like an admin panel.

## Horizontal privilege escalation

Horizontal privilege escalation happens when a user accesses another user’s data or resources instead of only their own. For example, by changing `myaccount?id=123` to `myaccount?id=124`, an attacker could view another user’s account.they might gain access to another user's account page, and the associated data and functions.

## Horizontal to vertical privilege escalation

Horizontal to vertical privilege escalation occurs when an attacker first takes over another user’s account and that user has higher privileges. For example, by changing `myaccount?id=456` to access an admin’s account and reset their password, the attacker can then gain full administrative access.

What is the difference between horizontal privilege and vertical privilege escalation ?

Horizontal privilege escalation means accessing other users’ data at the same permission level, like a student being able to view another student’s grades. Vertical privilege escalation means gaining higher permissions, like that same student accessing the teacher’s grading system and changing grades.


Lab 1 : 
https://youtu.be/qJ8mtm_G40E


Lab2 : 
https://youtu.be/7Jve11VySNU

Lab3 :
https://youtu.be/e_jsPdEeSto

Lab4 :
https://youtu.be/aaIfsH-fP5c

Lab5 :
https://youtu.be/tcPkT82pa6k