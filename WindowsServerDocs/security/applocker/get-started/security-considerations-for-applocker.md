---
title: Security Considerations for AppLocker
description: "Windows Server Security"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-applocker
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fe301ba-ef8f-4c26-8bdb-3f758af752ee
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
---
# Security Considerations for AppLocker

>Applies To: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

This topic for the IT professional describes the security considerations you need to address when implementing AppLocker.

The purpose of AppLocker is to restrict the access to software, and therefore, the data accessed by the software, to a specific group of users or within a defined business group. The following are security considerations for AppLocker:

-   AppLocker is deployed within an enterprise and administered centrally by those in IT with trusted credentials. This makes its policy creation and deployment conform to similar policy deployment processes and security restrictions.

-   AppLocker policies are distributed through known processes and by known means within the domain through Group Policy. But AppLocker policies can also be set on individual computers if the person has administrator privileges, and those policies might be contrary to the organization's written security policy. The enforcement settings for local policies are overridden by the same AppLocker policies in a Group Policy Object (GPO). However, because AppLocker rules are additive, a local policy that is not in a GPO will still be evaluated for that computer.

-   Microsoft does not provide a way to develop any extensions to AppLocker. The interfaces are not public. A user with administrator credentials can automate some AppLocker processes by using Windows PowerShell cmdlets. For information about the Windows PowerShell cmdlets for AppLocker, see the [AppLocker PowerShell Command Reference](https://technet.microsoft.com/en-us/library/ee424349(d=default,l=en-us,v=ws.10).aspx).

-   AppLocker runs in the context of Administrator or LocalSystem, which is the highest privilege set. This security context has the potential of misuse. If a user with administrative credentials makes changes to an AppLocker policy on a local computer that is joined to a domain, those changes could be overwritten or disallowed by the GPO that contains the AppLocker rule for the same file (or path) that was changed on the local computer. However, because AppLocker rules are additive, a local policy that is not in a GPO will still be evaluated for that computer. If the local computer is not joined to a domain and is not administered by Group Policy, a person with administrative credentials can alter the AppLocker policy.

-   When securing files in a directory with a rule of the path condition type, whether using the allow or deny action on the rule, it is still necessary and good practice to restrict access to those files by setting the access control lists (ACLs) according to your security policy.

-   AppLocker does not protect against running 16-bit DOS binaries in the Virtual DOS Machine (NTVDM). This technology allows running legacy DOS and 16-bit Windows programs on computers that are using Intel 80386 or later when there is already another operating system running and controlling the hardware. The result is that 16-bit binaries can still run on Windows Server 2008 R2 and Windows 7 when AppLocker is configured to otherwise block binaries and libraries. If it is a requirement to prevent 16-bit applications from running, you must configure the Deny rule in the Executable rule collection for NTVDM.exe.

-   You cannot use AppLocker (or Software Restriction Policies) to prevent code from running outside the Win32 subsystem. In particular, this applies to the (POSIX) subsystem in Windows NT. If it is a requirement to prevent applications from running in the POSIX subsystem, you must disable the subsystem. For more information, see [article 101270](http://support.microsoft.com/kb/101270) in the Microsoft Knowledge Base.

-   AppLocker can only control VBScript, JScript, .bat files, .cmd files, and Windows PowerShell scripts. It does not control all interpreted code that runs within a host process, for example, Perl scripts and macros. Interpreted code is a form of executable code that runs within a host process. For example, Windows batch files (*.bat) run within the context of the Windows Command Host (cmd.exe). To control interpreted code by using AppLocker, the host process must call AppLocker before it runs the interpreted code, and then enforce the decision returned by AppLocker. Not all host processes call into AppLocker and, therefore, AppLocker cannot control every kind of interpreted code, such as Microsoft Office macros.

    > [!IMPORTANT]
    > You should configure the appropriate security settings of these host processes if you must allow them to run. For example, configure the security settings in Microsoft Office to ensure that only signed and trusted macros are loaded.

-   AppLocker rules either allow or prevent an application from launching. AppLocker does not control the behavior of applications after they are launched. Applications could contain flags passed to functions that signal AppLocker to circumvent the rules and allow another .exe or .dll to be loaded. In practice, an application that is allowed by AppLocker could use these flags to bypass AppLocker rules and launch child processes. You must thoroughly examine each application before allowing them to run by using AppLocker rules.

    > [!NOTE]
    > Two flags that illustrate this condition are `SANDBOX_INERT`, which can be passed to `CreateRestrictedToken`, and `LOAD_IGNORE_CODE_AUTHZ_LEVEL`, which can be passed to `LoadLibraryEx`. Both of these flags signal AppLocker to circumvent the rules and allow a child .exe or .dll to be loaded.

-   In Windows 7, administrators have the option of restricting standard users from bypassing AppLocker policies when logging into their local computers in safe mode. For more information about this setting, see [article 977542](http://support.microsoft.com/kb/977542) in the Microsoft Knowledge Base.

## See Also
[AppLocker Technical Reference](applocker-technical-reference.md)

