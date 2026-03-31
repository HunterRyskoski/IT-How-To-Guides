# SOP: Create and Configure a Local User Account on a Windows PC

## Purpose
This procedure outlines how to create a local user account on a Windows PC, configure common account settings, optionally add the account to the local Administrators group, and verify that the account was set up correctly.

## Scope
Use this process when a device needs:
- a standard local user account
- a local administrative account
- a fallback/local support account
- a shared local account for a specific device function

## Requirements
- Administrator access on the PC
- PowerShell or Command Prompt opened **as Administrator**
- Approved username, full name, password, and intended permission level

---

## Procedure

### 1. Create the local account
Run the following command:

```powershell
net user "Username" "Password" /add /active:yes /fullname:"Full Name" /passwordchg:no
```

#### What this does
- Creates the local account
- Sets the password
- Makes the account active
- Sets the full name/display name
- Prevents the user from changing the password

---

### 2. Set the password to never expire
Run:

```powershell
Set-LocalUser -Name "Username" -PasswordNeverExpires 1
```

#### What this does
- Configures the local account so the password does not expire automatically

---

### 3. Ensure the account requires a password
Run:

```powershell
net user "Username" /passwordreq:yes
```

#### What this does
- Ensures the account must have a password
- Prevents the account from being treated as if a blank password is allowed

---

### 4. Add the account to the local Administrators group if needed
Only do this if the account is intended to be a local administrator.

```powershell
Add-LocalGroupMember -Group "Administrators" -Member "Username" -ErrorAction Stop
```

#### What this does
- Grants the account local administrator rights on that PC

If the account should be a standard local user, skip this step.

---

## Verification

### 5. Verify the account settings
Run:

```powershell
net user "Username"
```

Review the output and confirm the following fields.

#### Account active
- **Yes** = the account is enabled and can be used to sign in
- **No** = the account is disabled

#### Password expires
- **Never** = password expiration is disabled
- If a date or standard expiration behavior applies, the password is still subject to expiration rules

#### Password required
- **Yes** = the account must have a password
- **No** = Windows will allow the account to exist without requiring one, which is generally not preferred

#### User may change password
- **No** = the user cannot change the account password
- **Yes** = the user is allowed to change the password

#### Local Group Memberships
This shows what local group(s) the account belongs to.

Common examples:
- `*Users` = standard local user account
- `*Administrators` = local administrator account

---

### 6. Verify local administrator membership separately
If the account was meant to be a local administrator, confirm it with:

```powershell
net localgroup Administrators
```

#### Expected result
- The username should appear in the list if it was successfully added to the local Administrators group

---

## Interpretation of Key Fields

### Account active
Indicates whether the account is enabled for sign-in.

### Password expires
Indicates whether the password is subject to expiration. For service, support, or shared local accounts, this is often set to **Never** based on company standards.

### Password required
Indicates whether the account must have a password. This should generally be **Yes**.

### User may change password
Indicates whether the account itself can change its password. This is often set to **No** for controlled support or shared-use accounts.

### Local Group Memberships
Indicates the permission level of the account on the device:
- `Users` = standard permissions
- `Administrators` = elevated local admin permissions

---

## Example Outcomes

### Standard local user account
A standard local user account should typically show:
- Account active = **Yes**
- Password expires = **Never**
- Password required = **Yes**
- User may change password = **No**
- Local Group Memberships = `*Users`

### Local administrator account
A local administrator account should typically show:
- Account active = **Yes**
- Password expires = **Never**
- Password required = **Yes**
- User may change password = **No**
- Local Group Memberships = `*Administrators`



---

## Recommended Final Check
After setup, run:

```powershell
net user "Username"
```

Confirm:
- Account is active
- Password expires = Never
- Password required = Yes
- User may change password = No
- Local group membership matches intended role

If the account should be an admin, also run:

```powershell
net localgroup Administrators
```

And confirm the username is listed.
