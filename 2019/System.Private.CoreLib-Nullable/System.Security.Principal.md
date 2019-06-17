# System.Security.Principal

```diff
---D:\Temp\nullable\before
+++D:\Temp\nullable\after
 assembly System.Security.Principal {
  namespace System.Security.Principal {
    public interface IIdentity {
      string AuthenticationType { get; }
      bool IsAuthenticated { get; }
      string Name { get; }
    }
    public interface IPrincipal {
      IIdentity Identity { get; }
      bool IsInRole(string role);
    }
    public enum PrincipalPolicy {
      NoPrincipal = 1,
      UnauthenticatedPrincipal = 0,
      WindowsPrincipal = 2,
    }
    public enum TokenImpersonationLevel {
      Anonymous = 1,
      Delegation = 4,
      Identification = 2,
      Impersonation = 3,
      None = 0,
    }
  }
 }
```
