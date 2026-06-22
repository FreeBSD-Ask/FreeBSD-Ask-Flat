# 9.7 Root User Desktop Login

> **Warning**
>
> This section was written because some users wish to log in to the desktop using the root account. The root account has the highest system privileges, and improper use of the root account may cause system damage. Therefore, logging into the graphical interface with it carries extremely high security risks. The following operations should be performed with caution and at your own risk.

## File Structure

The relevant file structure covered in this section:

```sh
/usr/local/etc/
└── pam.d/
    ├── gdm-password # GDM login PAM configuration
    ├── lightdm      # LightDM login PAM configuration
    └── sddm         # SDDM login PAM configuration
```

## GDM

GDM, i.e., GNOME Display Manager.

Edit the configuration file **/usr/local/etc/pam.d/gdm-password** and comment out the `account requisite pam_securetty.so` line (add `#` at the beginning of the line).

Restart the GDM service:

```sh
# service gdm restart
```

## LightDM

LightDM, i.e., Light Display Manager.

Edit the **/usr/local/etc/pam.d/lightdm** file and comment out the `account requisite pam_securetty.so` line (add `#` at the beginning of the line).

Restart the LightDM service:

```sh
# service lightdm restart
```

## SDDM

SDDM, i.e., Simple Desktop Display Manager.

Modify the **/usr/local/etc/pam.d/sddm** file: change `login` after `include` to `system`, for a total of four occurrences.

Restart the SDDM service:

```sh
# service sddm restart
```

After restarting, you can log in to SDDM as root.

> **Warning**
>
> The root account has the highest privileges, and improper use of the root account may cause system damage. Therefore, logging into the graphical interface with it carries extremely high security risks.

## Exercises

1. Find the source code of the pam_securetty module and analyze its security design principles.
2. Analyze the difference between `include login` and `include system` in SDDM's PAM configuration, and understand why the latter allows root login.
3. Analyze the mechanism of `pam_securetty.so` in LightDM's PAM configuration.

> **Security Reminder**
>
> Exercises 2 and 3 are intended to understand the design principles of PAM security mechanisms, not to encourage bypassing security restrictions. In production environments, you should always follow the principle of least privilege and avoid logging into the desktop using the root account.
