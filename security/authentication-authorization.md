<p><a target="_blank" href="https://app.eraser.io/workspace/2RMHJkLbQMUFa154zCMY" id="edit-in-eraser-github-link"><img alt="Edit in Eraser" src="https://firebasestorage.googleapis.com/v0/b/second-petal-295822.appspot.com/o/images%2Fgithub%2FOpen%20in%20Eraser.svg?alt=media&amp;token=968381c8-a7e7-472a-8ed6-4a6626da5501"></a></p>

# Authentication
## What is authentication?
Authentication is the process of verifying a user's identity. It's about answering the question:  "Who are you?"

## Key concepts
### Credentials
Users provide information to prove their identity

- **Username and password**
- **Biometrics**: Fingerprints, facial recognition
- **Tokens**: Json Web Tokens (JWTs) or OAuth tokens
- **Client Certificates**: Digital certificates for secure communication
- **API Keys:** For machine-to-machine authentication
### Authentication Schemes
An "authentication scheme" is a named configuration that defines:

- How to **authenticate **a user (validate a cookie, decode a JWT)
- How to **challenge **an unauthenticated user (redirect to a login page for web apps, return 401 Unauthorized for APIs)
- How to forbid an unauthenticated user from accessing a resource they don't have permission for (return 403 Forbidden)
### Common built-in schemes:
- **Cookie Authentication**: Ideal for traditional web applications (MVC, Razor Pages) where sessions are managed via cookies
- **JWT Bearer Authentication**: Commonly used for APIs and SPA where token is sent with each request
- **OpenID Connect (OIDC)**: For integrating with external identity providers (Google, Facebook,...)
### **Claims & **`**ClaimsPrincipal**` 
After successful authentication, .NET creates a `ClaimsPrincipal `object, which represents the authenticated user and contains a collection of "claims"

**Claims**: a piece of information or a statement about the user. Examples:

- name: the user's full name
- email: the user's email
- sub: the subject identifier (unique ID) of user
- role: the roles user belongs to (e.g., "Admin", "Editor")
- permission: specific permissions the user has (e.g., "products.create", "users.delete")
Claims are fundamental as **authorization decision **(what a user is allowed to do) are often based on these claims

### **Authentication Handlers**
Each authentication scheme has an associated handler responsible for actual logic of processing credentials  and creating the `ClaimsPrincipal` 

## How Authentication Works in ASP.NET Core
The authentication process in .NET:

1. **Request arrives**: an HTTP request hits .NET application
2. `**UseAuthentication()**`** Middleware**: inspects the incoming request for credentials based on the configured authentication schemes
3. **Credential Validation**: an appropriate authentication handler attemps to validate the provided credentials
4. **ClaimsPrincipal Creatation**: if validation is successful, a `ClaimsPrincipal `object is constructed, populated with claims derived from the credentials, and attached to  `HttpContext.User` 
5. **Challenge (if unauthenticated)**: if no valid credentials are found or authentication fails, the authentication handler might issue a challenge
## Setting up Authentication
**Example: Cookie Authentication (for web applications)**

```
// Add authentication services
builder.Services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.LoginPath = "/Account/Login";     // Path to your login page
        options.LogoutPath = "/Account/Logout";   // Path to your logout action
        options.AccessDeniedPath = "/Account/AccessDenied"; // Optional: for 403 Forbidden
        options.Cookie.Name = "MyAppCookie";      // Name of the authentication cookie
        options.ExpireTimeSpan = TimeSpan.FromMinutes(30); // Cookie expiration
        options.SlidingExpiration = true;         // Renew cookie if used within half of expiry
        options.Cookie.HttpOnly = true;           // Prevent client-side script access
        options.Cookie.SecurePolicy = CookieSecurePolicy.Always; // Only send over HTTPS
        options.Cookie.SameSite = SameSiteMode.Lax; // Control cookie sending on cross-site requests
    });

```
**Example: JWT Bearer Authentication (for APIs/SPAs)**

```

builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        // Get JWT settings dynamically from IOptions<JwtOptions>
        var jwtOption = builder.Configuration.GetSection("JwtSettings").Get<JwtOptions>();

        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,

            ValidIssuer = jwtOption!.Issuer,
            ValidAudience = jwtOption!.Audience,
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(jwtOption.SecretKey))
        };

    });


```
# **Authorization** 
## What is Authorization?
**Authorization** is the process of determining whether an **authenticated user** has permission to perform a specific action or access a particular resource. It answers the crucial question: "What is this user allowed to do?"

Authorization always follows Authentication. You must first establish "who the user is" (authenticate) before you can determine "what they are allowed to do" (authorize).

## Key Concepts
## `[Authorize]`  attribute or `RequireAuthorization`   
The primary way to enforce authorization in .NET is using `[Authorize]`  attribute or `RequireAuthorization`  Can apply at different levels:

Controller level

Action Method level

Minimal API Endpoint

## `[AllowAnonymous]` attribute
This attribute explicitly bypasses authorization checks for a specific action or controller, is useful for endpoints that should be publicly accessible

### Authorization Policies
Policies are named sets of authorization requirements that encapsulate complex authorization logic 

### [﻿Claims & ClaimsPrincipal ﻿](https://app.eraser.io/workspace/2RMHJkLbQMUFa154zCMY#2q4x9Cdlcz_Qe1g6re0UH)﻿
## Types of Authorization in .NET
### Role-Based Authorization
This is the most straightforward method. Users are assigned to roles ("Admin", "User") and access is granted if the user possesses one of the required roles. Roles are represented as `ClaimTypes.Role` claims within the `ClaimsPrincipal` 

**Usage in Controller/Action:**

```
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("[controller]")]
public class ContentController : ControllerBase
{
    // Allows any authenticated user
    [HttpGet("view")]
    [Authorize]
    public IActionResult ViewContent() { /* ... */ }

    // Only users with the "Editor" role
    [HttpGet("edit")]
    [Authorize(Roles = "Editor")]
    public IActionResult EditContent() { /* ... */ }

    // Users with "Admin" OR "Publisher" role (comma-separated roles are OR)
    [HttpGet("publish")]
    [Authorize(Roles = "Admin, Publisher")]
    public IActionResult PublishContent() { /* ... */ }

    // Users must have "Admin" AND "Moderator" roles (multiple attributes are AND)
    [HttpGet("manage")]
    [Authorize(Roles = "Admin")]
    [Authorize(Roles = "Moderator")]
    public IActionResult ManageContent() { /* ... */ }
}
```
### Claims-Based Authorization
This approach makes authorization decisions based on any arbitrary claim type and value present in `ClaimsPrincipal`. It offers more granularity than roles

Configuration in `Program.cs`:

```
// Program.cs
// ...
builder.Services.AddAuthorization(options =>
{
    // Policy: User must have an "Email" claim
    options.AddPolicy("HasEmail", policy =>
        policy.RequireClaim(ClaimTypes.Email));

    // Policy: User must have a "Permission" claim with value "can_delete"
    options.AddPolicy("CanDelete", policy =>
        policy.RequireClaim("Permission", "can_delete")); // Custom claim type "Permission"

    // Policy: User must be from "Sales" OR "Marketing" department
    options.AddPolicy("SalesOrMarketing", policy =>
        policy.RequireClaim("Department", "Sales", "Marketing"));
});
// ...
```
**Usage in Controller/Action:**

```
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims; // For ClaimTypes

[ApiController]
[Route("[controller]")]
public class ClaimsBasedController : ControllerBase
{
    [HttpGet("my-email")]
    [Authorize(Policy = "HasEmail")] // Enforces the "HasEmail" policy
    public IActionResult GetMyEmail()
    {
        var email = User.FindFirst(ClaimTypes.Email)?.Value;
        return Ok($"Your email is: {email}");
    }

    [HttpGet("delete-item")]
    [Authorize(Policy = "CanDelete")] // Enforces the "CanDelete" policy
    public IActionResult DeleteItem(int id)
    {
        return Ok($"Item {id} deleted.");
    }

    [HttpGet("department-report")]
    [Authorize(Policy = "SalesOrMarketing")]
    public IActionResult GetDepartmentReport()
    {
        var department = User.FindFirst("Department")?.Value;
        return Ok($"Accessing report for department: {department}");
    }
}
```
### Policy-Based Authorization (Advanced)
This is the most flexible approach, allowing you to define complex authorization rules by combining multiple requirements and implementing custom logic within authorization handlers. It's ideal for scenarios that can't be covered by simple role or claims checks:

- Checking the value of a claim against a dynamic condition (e.g., "user's age > 21")
- Performing external lookups (e.g., database queries) to determine access
- Implementing resource-based authorization (e.g., "user can only modify content they own")
Steps:

1. **Define a Requirement:** Create a class that implements `IAuthorizationRequirement` 
2. **Define a Handler:** Create a class that implements `AuthorizationHandler<TRequirement>`   (where `TRequirement`   is your custom requirement). This handler contains the actual logic
3. **Register Policy and Handler:** In `Program.cs`  , define the policy and register your handler in the DI container.
**Example: Minimum Age Policy**

#### `MinimumAgeRequirement.cs` 
```
// Authorization/MinimumAgeRequirement.cs
using Microsoft.AspNetCore.Authorization;

public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; }
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```
`MinimumAgeHandler.cs` 

```
// Authorization/MinimumAgeHandler.cs
using Microsoft.AspNetCore.Authorization;
using System.Security.Claims;

public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        // Assume 'DateOfBirth' claim is provided by the authentication mechanism
        var dateOfBirthClaim = context.User.FindFirst(c => c.Type == ClaimTypes.DateOfBirth);

        if (dateOfBirthClaim == null || !DateTime.TryParse(dateOfBirthClaim.Value, out DateTime dateOfBirth))
        {
            context.Fail(); // User has no DoB claim or it's invalid
            return Task.CompletedTask;
        }

        var age = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth.Date > DateTime.Today.AddYears(-age)) age--; // Adjust for birthdays later in the year

        if (age >= requirement.MinimumAge)
        {
            context.Succeed(requirement); // Requirement met
        }
        else
        {
            context.Fail(); // Requirement not met
        }
        return Task.CompletedTask;
    }
}
```
**Register in** `Program.cs` 

```
// Program.cs
// ...
builder.Services.AddAuthentication(/* ... your scheme ... */); // Ensure authentication is configured

builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("MustBeOver21", policy =>
        policy.Requirements.Add(new MinimumAgeRequirement(21)));
});

// Register your custom authorization handler with the DI container
builder.Services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();

// ... app.UseAuthentication(), app.UseAuthorization()
```
#### Usage in Controller/Action
Your authentication system must add a `ClaimTypes.DateOfBirth` claim for this to work.

```
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("[controller]")]
[Authorize] // Requires authentication first
public class AgeRestrictedController : ControllerBase
{
    [HttpGet("club-entry")]
    [Authorize(Policy = "MustBeOver21")] // Requires the "MustBeOver21" policy
    public IActionResult EnterClub()
    {
        return Ok("Welcome to the club!");
    }
}
```




<!--- Eraser file: https://app.eraser.io/workspace/2RMHJkLbQMUFa154zCMY --->