# Active Directory User Administration and Password Reset Workflow

Day-to-day account administration for a Tier 1-2 support role. Everything here assumes you have been delegated the permission to do it. Follow your organization's policy and approval process, and record every change in the ticket.

## 1. Core concepts

| Term | Meaning |
| --- | --- |
| Domain | Administrative boundary for users, computers and policies |
| Domain controller (DC) | Server that authenticates users and holds the directory (Kerberos and LDAP) |
| OU (organizational unit) | Container used to organize objects and to scope Group Policy and delegation |
| Security group | Grants access to resources. Use groups, not individual users, for permissions |
| GPO (Group Policy Object) | Settings applied to users or computers in linked OUs |
| UPN and sAMAccountName | `user@domain` sign-in name and the legacy short sign-in name |
| Hybrid identity | On-premises AD synced to Microsoft Entra ID (Entra Connect or Cloud Sync) |

## 2. Identity verification before any account change

Never reset a password, unlock an account or change MFA based only on a request over email or chat. Use the documented procedure, for example:

1. Call the user back on the number stored in the HR or directory system, not on a number given in the request.
2. Verify at least two data points the requester should know and an attacker would not (employee ID plus manager name, or a verified self-service enrollment).
3. For requests made on behalf of someone else, require a manager's approval in the ticket.
4. For VIP or privileged accounts, follow the stricter procedure (additional approval, security team notification).

## 3. Password reset workflow

1. Verify identity (section 2) and open or link the ticket.
2. Find the account in Active Directory Users and Computers (ADUC) or with `Get-ADUser <name> -Properties LockedOut,PasswordExpired,PasswordLastSet,Enabled`.
3. Confirm the account is **enabled** and not disabled for another reason (offboarding, security hold). If disabled, stop and check why.
4. Reset the password: ADUC > right-click user > Reset Password, set a unique temporary password according to policy, and tick **User must change password at next logon**.
5. Unlock if needed: tick **Unlock account** on the same dialog.
6. Deliver the temporary password through the approved channel (verbal on the verified call, or the self-service tool). Never put it in the ticket or an email.
7. Tell the user about connected credentials that will now be stale: saved Wi-Fi, mapped drives, Outlook on mobile, Teams, VPN and password managers.
8. If the account is hybrid, allow time for sync and check password writeback if you use Self-Service Password Reset (SSPR).
9. Document: who was verified, how, the time and ticket number. Do not record the password.

```powershell
# Requires the ActiveDirectory (RSAT) module and delegated rights
Get-ADUser jdoe -Properties LockedOut,PasswordExpired,PasswordLastSet,Enabled |
    Select-Object SamAccountName,Enabled,LockedOut,PasswordExpired,PasswordLastSet

Unlock-ADAccount -Identity jdoe
Set-ADAccountPassword -Identity jdoe -Reset -NewPassword (Read-Host -AsSecureString "Temporary password")
Set-ADUser -Identity jdoe -ChangePasswordAtLogon $true
```

## 4. Account lockouts

1. Confirm the lockout (`LockedOut` attribute) and unlock after verifying identity.
2. Find the source. Lockouts repeat when something keeps sending the old password: a phone with an old password (Exchange ActiveSync or Outlook mobile), a mapped drive, a scheduled task, a service account, an old RDP session or a saved credential.
3. On the PDC emulator, look for Security event ID **4740** (account lockout). The `Caller Computer Name` field names the source. Failed logon events are **4625** on the source device and **4771/4776** on domain controllers.
4. Remove or update the stale credential, and ask the user to update password on all devices.
5. If lockouts continue from unknown sources, escalate to the security team: it can be a password-spray attempt.

Microsoft's Account Lockout and Management Tools (`LockoutStatus.exe`) can show lockout status across all domain controllers.

## 5. New user creation

1. Receive an approved request from HR or the manager: full name, title, department, manager, start date, location, access needs, equipment.
2. Create the account using the standard naming convention and place it in the correct OU. Prefer copying a role-based template account, or use group membership defined for the role.
3. Set the UPN, email attributes, manager, department and title.
4. Add to role-based security groups and distribution lists only as approved.
5. Assign licenses (Microsoft 365) directly or through licensing groups, and confirm the mailbox is created.
6. Set an initial password with **must change at next logon**, delivered by the approved method on the first day.
7. Prepare the device (see the device lifecycle checklist), set up MFA registration, and complete the ticket checklist.

## 6. Group membership and shared resources

- Add and remove users from **groups**, not by editing resource permissions directly. Record the approver.
- Use `Get-ADPrincipalGroupMembership jdoe` to list groups, and `Get-ADGroupMember <group>` to list members.
- Allow time for group changes to take effect: the user must sign out and in (or reboot) so the Kerberos ticket includes the new groups, and replication between DCs can take minutes.
- File share access involves two permission layers: share permissions and NTFS permissions. The most restrictive combined result applies.

## 7. Offboarding and disabling accounts

1. Act on an approved HR request, at the stated time.
2. **Disable** the account rather than delete it. Move it to the disabled-users OU and record the date.
3. Reset the password to a random value, remove from privileged groups and revoke sessions and tokens in Entra ID.
4. Convert the mailbox to shared or set forwarding and an out-of-office message, according to the manager's instructions.
5. Transfer OneDrive data to the manager and keep it per the retention policy.
6. Reclaim the license after the retention requirement is satisfied, and collect the equipment (see the device lifecycle checklist).
7. Delete the account only after the retention period defined by policy.

## 8. Troubleshooting sign-in and domain problems

| Symptom | Check |
| --- | --- |
| "The trust relationship between this workstation and the primary domain failed" | Machine account password out of sync. Confirm network and time, then rejoin the domain or run `Test-ComputerSecureChannel -Repair -Credential <admin>` |
| Cannot find domain controller | DNS on the client points to a DC or internal DNS, not a public resolver. `nslookup -type=SRV _ldap._tcp.dc._msdcs.<domain>` and `nltest /dsgetdc:<domain>` |
| Slow sign-in | Large profile or redirected folders, many GPOs, DC or DNS latency, logon scripts |
| Settings not applying | `gpupdate /force`, then `gpresult /r` or `gpresult /h report.html` to see applied and denied GPOs |
| Password expired on remote user | VPN sign-in with a cached account blocks expiry notifications. Use the self-service reset or reset it for them |
| Kerberos errors with time skew | Clock difference over 5 minutes breaks Kerberos. Sync with `w32tm /resync` |

## 9. Safety rules

- Use the least privilege and a separate admin account for administrative work. Do not do daily work as a domain admin.
- Never share, log or email passwords. Never use the same temporary password for different users.
- Make one change at a time, and note the before and after values in the ticket.
- If a request looks unusual (urgent executive request, outside caller, no ticket), pause and escalate to security.

## Reference commands

`Get-ADUser`, `Set-ADUser`, `Unlock-ADAccount`, `Set-ADAccountPassword`, `Get-ADGroupMember`, `Add-ADGroupMember`, `Search-ADAccount -LockedOut`, `Search-ADAccount -AccountDisabled`, `gpresult`, `gpupdate`, `nltest`, `dsregcmd /status`, `klist`, `w32tm`.
