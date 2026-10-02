# 🚀 TechTweet Complete Learning Guide
**Oct 2, 2026 → Dec 31, 2026**
**Everything You Need: Links, Code, Tasks, Tracking**

---

## 📋 TABLE OF CONTENTS
1. [Quick Links (Copy-Paste)](#quick-links)
2. [Daily Tasks by Week](#daily-tasks-by-week)
3. [All Video Links](#all-video-links)
4. [All Code Templates](#all-code-templates)
5. [GitHub Commands](#github-commands)
6. [Tracking Template](#tracking-template)

---

## 🔗 QUICK LINKS (Copy-Paste Instantly)

### Accounts to Create
- GitHub: https://github.com
- Google Sheets: https://sheets.google.com
- Medium (blog): https://medium.com
- Azure: https://azure.microsoft.com/free
- Udemy: https://www.udemy.com
- Visual Studio: https://visualstudio.microsoft.com
- Postman: https://www.postman.com/downloads

### Your Progress Tracker
- Google Sheet: https://docs.google.com/spreadsheets/d/1DpZY8TyRJ8SatF6hByQfNR24BoUggfxPaQipe09RHuM/edit?pli=1&gid=0#gid=0
- GitHub Repo: https://github.com/harrympotter26-dev/TechTweet-Learning

---
## BEFORE STARTING

# Windows/Mac - Download these
Visual Studio 2022 Community: https://visualstudio.microsoft.com/vs/community/
SQL Server Developer Edition: https://www.microsoft.com/en-us/sql-server/sql-server-downloads
Git: https://git-scm.com/download
Node.js & npm: https://nodejs.org/ (for Angular)


## ⏰ DAILY TASKS BY WEEK

WEEK 1: AUTHENTICATION & DATABASE
Oct 2-8, 2026 | Goal: Secure auth system for 10M users
MONDAY, OCT 2
MORNING (8-10 AM): SOLID Principles
Video Search:

text

YouTube Search 1: "Milan Jovanovic SOLID principles C#"
YouTube Search 2: "Code Maze SOLID C# tutorial"

Duration: 40-50 minutes
Creator: Milan Jovanović or Code Maze (both excellent)

Topics to Learn:
- Single Responsibility Principle (SRP)
- Open/Closed Principle (OCP)
- Liskov Substitution Principle (LSP)
- Interface Segregation Principle (ISP)
- Dependency Inversion Principle (DIP)
Coding Task: Implement SRP

Create file: Week1/SRP-Implementation.cs

csharp

// WRONG - Violates SRP (does too much)
public class UserManager
{
    public void Register(string username, string password)
    {
        // Validation
        if (string.IsNullOrEmpty(username)) 
            throw new Exception("Invalid");
        
        // Hash password
        string salt = GenerateSalt();
        string hash = HashPassword(password, salt);
        
        // Save to database
        var user = new User { Username = username, PasswordHash = hash };
        _db.Users.Add(user);
        _db.SaveChanges();
        
        // Send email
        SendWelcomeEmail(username);
    }
}

// RIGHT - Follows SRP (each class = one responsibility)

public class UserRepository
{
    private readonly DbContext _db;
    
    public async Task CreateUserAsync(User user)
    {
        _db.Users.Add(user);
        await _db.SaveChangesAsync();
    }
}

public class PasswordService
{
    public (string hash, string salt) HashPassword(string password)
    {
        using (var rng = new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            byte[] saltBytes = new byte[16];
            rng.GetBytes(saltBytes);
            
            var pbkdf2 = new System.Security.Cryptography.Rfc2898DeriveBytes(
                password,
                saltBytes,
                iterations: 10000,
                hashAlgorithm: System.Security.Cryptography.HashAlgorithmName.SHA256
            );
            
            byte[] hash = pbkdf2.GetBytes(32);
            return (
                Convert.ToBase64String(hash),
                Convert.ToBase64String(saltBytes)
            );
        }
    }
}

public class UserValidator
{
    public void ValidateUsername(string username)
    {
        if (string.IsNullOrEmpty(username))
            throw new ArgumentException("Username required");
        if (username.Length < 3)
            throw new ArgumentException("Username too short");
    }
}

public class EmailService
{
    public async Task SendWelcomeEmailAsync(string email, string username)
    {
        // Send email logic
    }
}

public class RegisterService
{
    private readonly UserValidator _validator;
    private readonly PasswordService _passwordService;
    private readonly UserRepository _repository;
    private readonly EmailService _emailService;
    
    public async Task RegisterUserAsync(string username, string password, string email)
    {
        _validator.ValidateUsername(username);
        var (hash, salt) = _passwordService.HashPassword(password);
        
        var user = new User 
        { 
            Username = username, 
            Email = email,
            PasswordHash = hash,
            Salt = salt 
        };
        
        await _repository.CreateUserAsync(user);
        await _emailService.SendWelcomeEmailAsync(email, username);
    }
}
Checklist:

 Code compiles without errors
 Each class has ONE responsibility
 Can test each class independently
 Saved to Week1/SRP-Implementation.cs
 Ready to commit
Git Commit:

Bash

git add .
git commit -m "Week1-Day1-Morning: SOLID principles - SRP implementation"
git push origin main
EVENING (9-10 PM): Password Hashing
Video Search:

text

YouTube Search: "Computerphile password hashing PBKDF2"
OR "Nick Chapsas password hashing C#"

Duration: 20-30 minutes
Creator: Computerphile or Nick Chapsas

Topics:
- Why NOT plain text
- Why NOT MD5/SHA1
- PBKDF2: Purpose-built for passwords
- Salt: Random value added before hashing
- Iterations: 10,000+ (intentionally SLOW)
- Why slow is GOOD: Makes brute force expensive
Coding Task: Password Verification

Add to: Week1/PasswordService.cs

csharp

public class PasswordService
{
    // Existing HashPassword from morning...
    
    public bool VerifyPassword(string inputPassword, string storedHash, string storedSalt)
    {
        var saltBytes = Convert.FromBase64String(storedSalt);
        var pbkdf2 = new System.Security.Cryptography.Rfc2898DeriveBytes(
            inputPassword,
            saltBytes,
            iterations: 10000,
            hashAlgorithm: System.Security.Cryptography.HashAlgorithmName.SHA256
        );
        
        byte[] hash = pbkdf2.GetBytes(32);
        string computedHash = Convert.ToBase64String(hash);
        
        return computedHash == storedHash;
    }
}
Checklist:

 Can hash password
 Can verify matching password
 Returns false for wrong password
 Ready to commit
Git Commit:

Bash

git add .
git commit -m "Week1-Day1-Evening: Password verification with PBKDF2"
git push origin main
Daily Progress:

Morning hours: 120 min ✓
Evening hours: 60 min ✓
Total: 180 min ✓
Commits: 2
Files created: 2
Motivation: ___/10
Blocker: None / __________
Tomorrow focus: JWT authentication
TUESDAY, OCT 3
MORNING (8-10 AM): JWT Authentication
Video Search:

text

YouTube Search 1: "Milan Jovanovic JWT authentication ASP.NET Core"
YouTube Search 2: "Hussein Nasser JWT explained"

Duration: 40-50 minutes
Creator: Milan Jovanović or Hussein Nasser

Topics:
- JWT Structure: Header.Payload.Signature
- Why stateless > sessions at scale
- 10K users with sessions = 50K DB calls/sec (CRASHES!)
- 10K users with JWT = 0 DB calls (SCALES!)
- Claims, token generation, signature verification
Coding Task: JWT Login Endpoint

Create file: Week1/AuthController.cs

csharp

using Microsoft.AspNetCore.Mvc;
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using Microsoft.IdentityModel.Tokens;
using System.Text;

[ApiController]
[Route("api/[controller]")]
public class AuthController : ControllerBase
{
    private readonly IConfiguration _config;
    private readonly PasswordService _passwordService;
    private readonly UserRepository _userRepository;
    
    public AuthController(
        IConfiguration config,
        PasswordService passwordService,
        UserRepository userRepository)
    {
        _config = config;
        _passwordService = passwordService;
        _userRepository = userRepository;
    }
    
    [HttpPost("login")]
    public async Task<IActionResult> Login([FromBody] LoginRequest request)
    {
        // Step 1: Validate input
        if (string.IsNullOrEmpty(request.Username) || string.IsNullOrEmpty(request.Password))
            return BadRequest("Username and password required");
        
        // Step 2: Find user in database
        var user = await _userRepository.GetUserByUsernameAsync(request.Username);
        if (user == null)
            return Unauthorized("Invalid username or password");
        
        // Step 3: Verify password
        bool passwordValid = _passwordService.VerifyPassword(
            request.Password,
            user.PasswordHash,
            user.Salt
        );
        
        if (!passwordValid)
            return Unauthorized("Invalid username or password");
        
        // Step 4: Create JWT token
        var token = GenerateJwtToken(user);
        
        // Step 5: Return token to client
        return Ok(new
        {
            token = token,
            username = user.Username,
            expiresIn = "24 hours"
        });
    }
    
    private string GenerateJwtToken(User user)
    {
        // Get secret from appsettings.json
        var secret = _config["Jwt:Secret"];
        var key = Encoding.ASCII.GetBytes(secret);
        
        // Create security key
        var securityKey = new SymmetricSecurityKey(key);
        var credentials = new SigningCredentials(
            securityKey,
            SecurityAlgorithms.HmacSha256Signature
        );
        
        // Create claims (data inside token)
        var claims = new[]
        {
            new Claim("UserId", user.UserId.ToString()),
            new Claim("Username", user.Username),
            new Claim("Email", user.Email),
            new Claim(ClaimTypes.Role, "User")
        };
        
        // Create token descriptor
        var tokenDescriptor = new SecurityTokenDescriptor
        {
            Subject = new ClaimsIdentity(claims),
            Expires = DateTime.UtcNow.AddHours(24),
            SigningCredentials = credentials
        };
        
        // Generate token
        var tokenHandler = new JwtSecurityTokenHandler();
        var token = tokenHandler.CreateToken(tokenDescriptor);
        var tokenString = tokenHandler.WriteToken(token);
        
        return tokenString;
    }
}

public class LoginRequest
{
    public string Username { get; set; }
    public string Password { get; set; }
}
Update appsettings.json:

JSON

{
  "Jwt": {
    "Secret": "your-very-long-secret-key-min-32-characters-keep-it-safe!"
  }
}
Update Startup (Program.cs or Startup.cs):

csharp

// In ConfigureServices or builder.Services
var jwtSecret = configuration["Jwt:Secret"];
var key = Encoding.ASCII.GetBytes(jwtSecret);

services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddJwtBearer(options =>
{
    options.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuerSigningKey = true,
        IssuerSigningKey = new SymmetricSecurityKey(key),
        ValidateIssuer = false,
        ValidateAudience = false,
        ValidateLifetime = true
    };
});

// In Configure
app.UseAuthentication();
app.UseAuthorization();
Test with Postman:

text

Method: POST
URL: http://localhost:5000/api/auth/login
Body (JSON):
{
  "username": "testuser",
  "password": "TestPassword123"
}

Expected Response:
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "username": "testuser",
  "expiresIn": "24 hours"
}
Checklist:

 Endpoint compiles
 Can login with username/password
 Returns JWT token
 Token has expiration
 Can test with Postman
 Ready to commit
Git Commit:

Bash

git add .
git commit -m "Week1-Day2-Morning: JWT login endpoint implementation"
git push origin main
EVENING (9-10 PM): Token Revocation
Video Search:

text

YouTube Search: "JWT token revocation logout"
OR "JWT token blacklist Redis"

Duration: 20-25 minutes
Creator: Hussein Nasser or Code Maze

Topics:
- Problem: Token valid for 24 hours even after logout!
- Solution: Blacklist tokens in Redis
- Real scenario: Token stolen, but user logged out
- Blacklist checks: <1ms per request
Coding Task: Token Blacklist

Create file: Week1/TokenBlacklistService.cs

csharp

using StackExchange.Redis;
using System;
using System.Threading.Tasks;

public class TokenBlacklistService
{
    private readonly IDatabase _cache;
    
    public TokenBlacklistService(IConnectionMultiplexer redis)
    {
        _cache = redis.GetDatabase();
    }
    
    // When user logs out, add token to blacklist
    public async Task BlacklistTokenAsync(string token, DateTime expiration)
    {
        var timeUntilExpiry = expiration - DateTime.UtcNow;
        
        if (timeUntilExpiry.TotalSeconds > 0)
        {
            await _cache.StringSetAsync(
                $"blacklist:{token}",
                "true",
                timeUntilExpiry
            );
        }
    }
    
    // Check if token is blacklisted
    public async Task<bool> IsTokenBlacklistedAsync(string token)
    {
        var value = await _cache.StringGetAsync($"blacklist:{token}");
        return value.HasValue;
    }
}
Add Logout Endpoint to AuthController:

csharp

[HttpPost("logout")]
[Authorize]
public async Task<IActionResult> Logout()
{
    // Get token from header
    var token = Request.Headers["Authorization"]
        .ToString()
        .Replace("Bearer ", "");
    
    // Get expiration from JWT
    var handler = new JwtSecurityTokenHandler();
    var jwtToken = handler.ReadJwtToken(token);
    
    // Add to blacklist
    await _tokenBlacklistService.BlacklistTokenAsync(
        token,
        jwtToken.ValidTo
    );
    
    return Ok("Logged out successfully");
}
Checklist:

 Blacklist service compiles
 Logout endpoint created
 Token added to Redis blacklist
 Ready to commit
Git Commit:

Bash

git add .
git commit -m "Week1-Day2-Evening: Token blacklist + logout functionality"
git push origin main
Daily Progress:

Total: 180 min ✓
Commits: 2
Motivation: ___/10
Tomorrow focus: Database schema and indexing
WEDNESDAY, OCT 4
MORNING (8-10 AM): Database Indexing
Video Search:

text

YouTube Search 1: "Hussein Nasser database indexing explained"
YouTube Search 2: "Brent Ozar SQL Server indexing deep dive"

Duration: 40-50 minutes
Creator: Hussein Nasser or Brent Ozar

Topics:
- What is index: Sorted list of values
- B-Tree structure
- Clustered vs non-clustered
- Query: 30 seconds WITHOUT index
- Same query: 50ms WITH index (600x faster!)
Coding Task: Database Schema

Create file: Week1/Database/Schema.sql

Open SSMS and run:

SQL

-- Create Database
CREATE DATABASE TechTweet;
USE TechTweet;

-- Create Users Table
CREATE TABLE Users (
    UserId BIGINT PRIMARY KEY IDENTITY(1,1),
    Username NVARCHAR(50) UNIQUE NOT NULL,
    Email NVARCHAR(100) UNIQUE NOT NULL,
    PasswordHash NVARCHAR(MAX) NOT NULL,
    Salt NVARCHAR(MAX) NOT NULL,
    FirstName NVARCHAR(100),
    LastName NVARCHAR(100),
    ProfilePictureUrl NVARCHAR(500),
    BioDescription NVARCHAR(500),
    CreatedAt DATETIME2 DEFAULT GETUTCDATE(),
    UpdatedAt DATETIME2,
    IsActive BIT DEFAULT 1,
    IsDeleted BIT DEFAULT 0
);

-- Add indexes
CREATE INDEX idx_username ON Users(Username);
CREATE INDEX idx_email ON Users(Email);
CREATE INDEX idx_createdAt ON Users(CreatedAt DESC);
CREATE INDEX idx_isActive ON Users(IsActive);

-- Create Tweets Table
CREATE TABLE Tweets (
    TweetId BIGINT PRIMARY KEY IDENTITY(1,1),
    UserId BIGINT NOT NULL,
    Content NVARCHAR(280) NOT NULL,
    CreatedAt DATETIME2 DEFAULT GETUTCDATE(),
    UpdatedAt DATETIME2,
    IsDeleted BIT DEFAULT 0,
    LikeCount INT DEFAULT 0,
    RetweetCount INT DEFAULT 0,
    ReplyCount INT DEFAULT 0,
    
    FOREIGN KEY (UserId) REFERENCES Users(UserId)
);

-- Add indexes
CREATE INDEX idx_userId_createdAt ON Tweets(UserId, CreatedAt DESC);
CREATE INDEX idx_createdAt ON Tweets(CreatedAt DESC);
CREATE INDEX idx_isDeleted ON Tweets(IsDeleted);
Checklist:

 Database created in SSMS
 Users table created
 Tweets table created
 All indexes added
 Can see both tables in SSMS
 SQL saved to file
Git Commit:

Bash

git add .
git commit -m "Week1-Day3-Morning: Database schema with indexes"
git push origin main
EVENING (9-10 PM): Query Optimization
Video Search:

text

YouTube Search: "SQL Server execution plans explained"
OR "Brent Ozar read execution plan"

Duration: 20-30 minutes
Creator: Brent Ozar or Hussein Nasser

Topics:
- How to enable execution plans in SSMS
- Green = good (Index Seek)
- Red = slow (Table Scan)
- Identifying missing indexes
Coding Task: Test Query Performance

In SSMS:

SQL

-- Enable execution plan
-- Query → Include Actual Execution Plan (Ctrl+L)

-- SLOW QUERY (no optimization)
SELECT * FROM Tweets 
WHERE Content LIKE '%hello%'

-- Add full-text index
CREATE FULLTEXT CATALOG ft_catalog;
CREATE FULLTEXT INDEX ON Tweets(Content) 
    KEY INDEX PK__Tweets__0
    ON ft_catalog;

-- FAST QUERY (optimized)
SELECT * FROM Tweets 
WHERE CONTAINS(Content, 'hello')
Performance improvement: 20-100x faster!

Checklist:

 Can enable execution plans in SSMS
 Can read execution plan results
 Understand Table Scan vs Index Seek
 Know why indexes matter at scale
Git Commit:

Bash

git add .
git commit -m "Week1-Day3-Evening: Query optimization with execution plans"
git push origin main
Daily Progress:

Total: 180 min ✓
Database: Ready ✓
Motivation: ___/10
Tomorrow focus: OAuth2 + 2FA
THURSDAY, OCT 5
MORNING (8-10 AM): OAuth2
Video Search:

text

YouTube Search 1: "TechWorld with Nana OAuth2 explained"
YouTube Search 2: "ByteByteGo OAuth 2.0 architecture"

Duration: 35-45 minutes
Creator: TechWorld with Nana or ByteByteGo

Topics:
- What is OAuth2: "Login with Google", "Login with GitHub"
- You don't store passwords
- Google authenticates user
- You get access token
- User logged in!
Coding Task: Google OAuth2

csharp

// Step 1: Go to https://console.cloud.google.com
// Create new project: "TechTweet"
// Enable Google+ API
// Create OAuth credentials:
//   - Type: Web application
//   - Authorized redirect: http://localhost:5000/api/auth/google-callback
// Copy: Client ID and Client Secret

// Step 2: Update appsettings.json
/*
{
  "Google": {
    "ClientId": "YOUR-CLIENT-ID.apps.googleusercontent.com",
    "ClientSecret": "YOUR-CLIENT-SECRET"
  }
}
*/

// Step 3: Install NuGet
// Install-Package Microsoft.AspNetCore.Authentication.Google

// Step 4: Configure Startup
services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = "Cookies";
    options.DefaultChallengeScheme = "Google";
})
.AddCookie("Cookies")
.AddGoogle(googleOptions =>
{
    googleOptions.ClientId = configuration["Google:ClientId"];
    googleOptions.ClientSecret = configuration["Google:ClientSecret"];
    googleOptions.SaveTokens = true;
});

app.UseAuthentication();
app.UseAuthorization();

// Step 5: Add endpoints to AuthController
[HttpGet("login/google")]
public IActionResult LoginWithGoogle()
{
    var redirectUrl = Url.Action("GoogleCallback", "Auth");
    var properties = new AuthenticationProperties { RedirectUri = redirectUrl };
    return Challenge(properties, "Google");
}

[HttpGet("google-callback")]
public async Task<IActionResult> GoogleCallback()
{
    var result = await HttpContext.AuthenticateAsync("Google");
    if (!result.Succeeded)
        return BadRequest("Google authentication failed");
    
    var email = result.Principal.FindFirst(ClaimTypes.Email)?.Value;
    var name = result.Principal.FindFirst(ClaimTypes.Name)?.Value;
    
    var user = await _userRepository.GetUserByEmailAsync(email);
    if (user == null)
    {
        user = new User
        {
            Email = email,
            Username = name,
            IsActive = true,
            CreatedAt = DateTime.UtcNow
        };
        await _userRepository.CreateUserAsync(user);
    }
    
    var token = GenerateJwtToken(user);
    return Ok(new { token = token, email = email, username = user.Username });
}
Checklist:

 Google app created
 Client ID and Secret saved
 NuGet package installed
 Startup configured
 Login endpoint working
 Can login with Google
Git Commit:

Bash

git add .
git commit -m "Week1-Day4-Morning: Google OAuth2 implementation"
git push origin main
EVENING (9-10 PM): 2FA/TOTP
Video Search:

text

YouTube Search: "Hussein Nasser TOTP two factor authentication"
OR "Google Authenticator how it works"

Duration: 20-25 minutes
Creator: Hussein Nasser or Code Maze

Topics:
- Something you KNOW: Password
- Something you HAVE: Phone with app
- Two factors = double security
- TOTP: Time-based OTP (most secure)
Coding Task: 2FA Implementation

csharp

// Install NuGet
// Install-Package OtpNet

using OtpNet;

public class TwoFactorService
{
    public string GenerateSecret()
    {
        byte[] randomBytes = new byte[32];
        using (var rng = new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }
        return Base32Encoding.ToString(randomBytes);
    }
    
    public bool VerifyTotp(string secret, string code)
    {
        var totp = new Totp(Base32Encoding.ToBytes(secret));
        return totp.VerifyTotp(code, out var window, VerificationWindow.RfcSpecifiedWindow);
    }
    
    public string GenerateQrCode(string secret, string userEmail, string appName)
    {
        var totp = new Totp(Base32Encoding.ToBytes(secret));
        var otherTotp = totp.GetOtherTotp();
        return otherTotp.ToUrl($"{appName} ({userEmail})");
    }
}

// Add endpoints to AuthController
[HttpPost("2fa/setup")]
[Authorize]
public IActionResult Setup2FA()
{
    var userId = User.FindFirst("UserId").Value;
    string secret = _twoFactorService.GenerateSecret();
    string qrCodeUri = _twoFactorService.GenerateQrCode(secret, "user@email.com", "TechTweet");
    
    return Ok(new { secret = secret, qrCodeUri = qrCodeUri });
}

[HttpPost("2fa/verify")]
[Authorize]
public IActionResult Verify2FA([FromBody] Verify2FARequest request)
{
    bool isValid = _twoFactorService.VerifyTotp(request.Secret, request.Code);
    if (!isValid)
        return BadRequest("Invalid code");
    
    return Ok(new { message = "2FA enabled successfully" });
}

public class Verify2FARequest
{
    public string Secret { get; set; }
    public string Code { get; set; }
}
Checklist:

 2FA service compiles
 Can generate secret
 Can verify codes
 QR code generation works
Git Commit:

Bash

git add .
git commit -m "Week1-Day4-Evening: 2FA with TOTP authentication"
git push origin main
Daily Progress:

Total: 180 min ✓
OAuth2: Working ✓
2FA: Working ✓
Motivation: ___/10
Tomorrow focus: Review + Blog post
FRIDAY, OCT 6
MORNING (8-10 AM): Self-Assessment
Quiz - Write Answers (no looking back!)

text

Q1: Why do we salt passwords?
A: ___________________________________________

Q2: What's better at scale: JWT or Sessions? Why?
A: ___________________________________________

Q3: Draw OAuth2 flow (5 steps)
A: ___________________________________________

Q4: What does an index do in a database?
A: ___________________________________________

Q5: How does TOTP work?
A: ___________________________________________

Q6: Can someone forge a JWT token? How to prevent?
A: ___________________________________________

Q7: What is SRP?
A: ___________________________________________

Score: ___/7 correct
Confidence: ___/10

If < 6/7: Review topics again
If = 7/7: Ready for Week 2! ✓
MORNING (8-10 AM): Code Review
Checklist All Week 1 Code:

text

Security:
- [ ] No hardcoded secrets
- [ ] HTTPS will be used in production
- [ ] SQL injection protected
- [ ] Passwords never logged
- [ ] Rate limiting considered

Code Quality:
- [ ] Each class has ONE responsibility
- [ ] Clear naming
- [ ] Comments on WHY not WHAT
- [ ] Can understand 6 months later

Database:
- [ ] Tables created
- [ ] Indexes added
- [ ] Foreign keys defined
- [ ] Queries optimized

If all checked: READY FOR WEEK 2! ✓
Git Commit:

Bash

git add .
git commit -m "Week1: Complete - Auth system + Database ready"
git push origin main
EVENING (9-10 PM): Blog Post
Write & Publish:

Title: "Building Secure Authentication for 10M Users"

Sections:

Introduction (100 words) - Why auth matters at scale
Password Security (300 words) - Hashing, salting, iterations
JWT vs Sessions (300 words) - Why stateless scales
OAuth2 (250 words) - Third-party authentication
2FA/TOTP (250 words) - Multi-factor security
Best Practices (200 words) - HTTPS, token expiration, rate limiting
Real Numbers (150 words) - Performance metrics at 10M users
Publish on: Medium, Dev.to, or Hashnode

Git Commit:

Bash

git add .
git commit -m "Week1-Day5: Blog post on secure authentication published"
git push origin main
Week 1 Summary:

text

✅ WEEK 1 COMPLETE!

What You Built:
✓ Secure password hashing
✓ JWT authentication
✓ Google OAuth2
✓ 2FA with TOTP
✓ Database schema with indexes
✓ Query optimization
✓ 30+ unit tests

Metrics:
- Hours: 15/15 ✓
- Commits: 10+
- Blog posts: 1/1 ✓

Skills Improved:
- C#: 3/5 → 3.5/5
- .NET: 3/5 → 3.5/5
- SQL: 2/5 → 2.8/5
- Security: 0/5 → 3/5

Next Week: Message Queues & Caching!
WEEK 2: MESSAGE QUEUES & CACHING
Oct 9-15, 2026 | Goal: Handle 1M tweets/second
MONDAY, OCT 9
MORNING (8-10 AM): Message Queues
Video Search:

text

YouTube Search: "Hussein Nasser message queues explained"
OR "ByteByteGo message queue architecture"

Duration: 40-50 minutes
Creator: Hussein Nasser or ByteByteGo

Topics:
- Problem: 1M concurrent writes crash database
- Solution: Message queues decouple writes
- Producer adds to queue (instant)
- Consumer processes asynchronously
- Database handles manageable load
Coding Task: Implement Tweet Queue

Create file: Week2/TweetQueue.cs

csharp

// Install NuGet
// Install-Package Azure.Messaging.ServiceBus

using Azure.Messaging.ServiceBus;
using System.Text.Json;

public class TweetService
{
    private readonly ServiceBusClient _serviceBusClient;
    private readonly AppDbContext _db;
    
    public TweetService(ServiceBusClient serviceBusClient, AppDbContext db)
    {
        _serviceBusClient = serviceBusClient;
        _db = db;
    }
    
    // When user creates tweet: send to queue (don't wait for DB)
    public async Task<string> CreateTweetAsync(long userId, string content)
    {
        // Create message
        var message = new TweetCreatedMessage
        {
            UserId = userId,
            Content = content,
            CreatedAt = DateTime.UtcNow
        };
        
        // Send to queue
        var sender = _serviceBusClient.CreateSender("tweet-creation");
        var serviceBusMessage = new ServiceBusMessage(
            JsonSerializer.Serialize(message)
        );
        
        await sender.SendMessageAsync(serviceBusMessage);
        await sender.DisposeAsync();
        
        // Return immediately (< 100ms)
        return "Tweet submitted for processing";
    }
}

// Controller endpoint
[HttpPost("tweets")]
[Authorize]
public async Task<IActionResult> CreateTweet([FromBody] CreateTweetRequest request)
{
    var userId = long.Parse(User.FindFirst("UserId").Value);
    
    if (string.IsNullOrEmpty(request.Content) || request.Content.Length > 280)
        return BadRequest("Tweet must be 1-280 characters");
    
    var result = await _tweetService.CreateTweetAsync(userId, request.Content);
    return Accepted(new { message = result });
}

public class CreateTweetRequest
{
    public string Content { get; set; }
}

public class TweetCreatedMessage
{
    public long UserId { get; set; }
    public string Content { get; set; }
    public DateTime CreatedAt { get; set; }
}
Checklist:

 Azure Service Bus setup
 Message published to queue
 API returns immediately (< 100ms)
Git Commit:

Bash

git add .
git commit -m "Week2-Day1-Morning: Tweet creation with message queue"
git push origin main
EVENING (9-10 PM): Background Workers
Video Search:

text

YouTube Search: "Hosted services background workers ASP.NET Core"

Duration: 20-25 minutes

Topics:
- Background workers process queue asynchronously
- Multiple workers can run in parallel
- Database handles manageable load
- Retry logic for failed messages
Coding Task: Queue Consumer

Create file: Week2/TweetProcessingWorker.cs

csharp

using Azure.Messaging.ServiceBus;
using Microsoft.Extensions.Hosting;
using System.Text.Json;

public class TweetProcessingWorker : BackgroundService
{
    private readonly ServiceBusClient _serviceBusClient;
    private readonly AppDbContext _db;
    private readonly ILogger<TweetProcessingWorker> _logger;
    
    public TweetProcessingWorker(
        ServiceBusClient serviceBusClient,
        AppDbContext db,
        ILogger<TweetProcessingWorker> logger)
    {
        _serviceBusClient = serviceBusClient;
        _db = db;
        _logger = logger;
    }
    
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        var processor = _serviceBusClient.CreateProcessor(
            "tweet-creation",
            new ServiceBusProcessorOptions()
        );
        
        processor.ProcessMessageAsync += ProcessMessageAsync;
        processor.ProcessErrorAsync += ProcessErrorAsync;
        
        await processor.StartProcessingAsync(stoppingToken);
        
        while (!stoppingToken.IsCancellationRequested)
        {
            await Task.Delay(1000, stoppingToken);
        }
        
        await processor.StopProcessingAsync();
        await processor.DisposeAsync();
    }
    
    private async Task ProcessMessageAsync(ProcessMessageEventArgs args)
    {
        try
        {
            var message = args.Message;
            var tweetMessage = JsonSerializer.Deserialize<TweetCreatedMessage>(
                message.Body.ToString()
            );
            
            _logger.LogInformation("Processing tweet from user {UserId}", tweetMessage.UserId);
            
            // Step 1: Save to database
            var tweet = new Tweet
            {
                UserId = tweetMessage.UserId,
                Content = tweetMessage.Content,
                CreatedAt = tweetMessage.CreatedAt
            };
            
            _db.Tweets.Add(tweet);
            await _db.SaveChangesAsync();
            
            _logger.LogInformation("Tweet {TweetId} saved", tweet.TweetId);
            
            // Step 2: Update cache
            // await _cacheService.InvalidateUserFeed(tweetMessage.UserId);
            
            // Step 3: Send notifications
            // await _notificationService.NotifyFollowers(tweetMessage.UserId, tweet);
            
            // Complete message (remove from queue)
            await args.CompleteMessageAsync(message);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error processing tweet message");
            await args.DeadLetterMessageAsync(args.Message);
        }
    }
    
    private Task ProcessErrorAsync(ProcessErrorEventArgs args)
    {
        _logger.LogError(
            args.Exception,
            "Error processing Service Bus message"
        );
        return Task.CompletedTask;
    }
}
Register in Startup:

csharp

// In ConfigureServices
services.AddHostedService<TweetProcessingWorker>();
Checklist:

 Worker compiles
 Processes messages from queue
 Saves to database
 Handles errors with dead-letter queue
Git Commit:

Bash

git add .
git commit -m "Week2-Day1-Evening: Background worker for tweet processing"
git push origin main
TUESDAY, OCT 10
MORNING (8-10 AM): Database Transactions
Video Search:

text

YouTube Search: "Hussein Nasser database isolation levels"
OR "ACID properties explained"

Duration: 40-50 minutes
Creator: Hussein Nasser

Topics:
- ACID: Atomicity, Consistency, Isolation, Durability
- Isolation levels: Read Committed, Repeatable Read, Serializable
- Problem: Multiple transactions interfering
- Solution: Proper transaction handling
Coding Task: Implement Transactions

Create file: Week2/TransactionExample.cs

csharp

public class BankTransferService
{
    private readonly AppDbContext _db;
    
    // WRONG - Not atomic (can fail mid-transfer)
    public async Task TransferMoneyBad(int fromUserId, int toUserId, decimal amount)
    {
        var fromUser = await _db.Users.FindAsync(fromUserId);
        fromUser.Balance -= amount;
        await _db.SaveChangesAsync();
        
        // If system crashes HERE, money is lost!
        
        var toUser = await _db.Users.FindAsync(toUserId);
        toUser.Balance += amount;
        await _db.SaveChangesAsync();
    }
    
    // RIGHT - Atomic transaction (all or nothing)
    public async Task TransferMoneyGood(int fromUserId, int toUserId, decimal amount)
    {
        using (var transaction = await _db.Database.BeginTransactionAsync())
        {
            try
            {
                // Step 1: Debit from sender
                var fromUser = await _db.Users.FindAsync(fromUserId);
                if (fromUser.Balance < amount)
                    throw new InvalidOperationException("Insufficient funds");
                fromUser.Balance -= amount;
                
                // Step 2: Credit to receiver
                var toUser = await _db.Users.FindAsync(toUserId);
                toUser.Balance += amount;
                
                // Step 3: Log transaction
                var log = new TransactionLog
                {
                    FromUserId = fromUserId,
                    ToUserId = toUserId,
                    Amount = amount,
                    CreatedAt = DateTime.UtcNow,
                    Status = "completed"
                };
                _db.TransactionLogs.Add(log);
                
                // All or nothing
                await _db.SaveChangesAsync();
                await transaction.CommitAsync();
            }
            catch (Exception ex)
            {
                // If ANY step fails, ROLLBACK everything
                await transaction.RollbackAsync();
                throw;
            }
        }
    }
}
Checklist:

 Transactions compile
 Understand ACID properties
 Know what happens on rollback
Git Commit:

Bash

git add .
git commit -m "Week2-Day2-Morning: Database transactions ACID implementation"
git push origin main
EVENING (9-10 PM): Transaction Isolation
Video Search:

text

YouTube Search: "SQL Server transaction isolation levels explained"

Duration: 20-25 minutes

Topics:
- Read Uncommitted (dirty reads)
- Read Committed (default)
- Repeatable Read (consistent reads)
- Serializable (safest, slowest)
Coding Task: Test Isolation

Create file: Week2/IsolationExample.sql

SQL

-- Test different isolation levels

-- Level 1: Dirty Read (AVOID!)
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT LikeCount FROM Tweets WHERE TweetId = 123;
-- Can see uncommitted changes from other transactions
COMMIT;

-- Level 2: Read Committed (DEFAULT - most common)
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;
SELECT LikeCount FROM Tweets WHERE TweetId = 123;
-- Can see committed changes only
COMMIT;

-- Level 3: Repeatable Read (SAFE)
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SELECT LikeCount FROM Tweets WHERE TweetId = 123;
WAITFOR DELAY '00:00:05';  -- Wait 5 seconds
SELECT LikeCount FROM Tweets WHERE TweetId = 123;
-- Second select returns SAME value (not updated by others)
COMMIT;

-- Level 4: Serializable (SAFEST, SLOWEST)
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SELECT * FROM Tweets WHERE UserId = 456;
-- Other transactions can't insert/update this user's tweets
-- Prevents phantom reads
COMMIT;
Checklist:

 Understand isolation levels
 Know trade-offs (safety vs. performance)
 Know when to use each
Git Commit:

Bash

git add .
git commit -m "Week2-Day2-Evening: Transaction isolation levels testing"
git push origin main
WEDNESDAY, OCT 11
MORNING (8-10 AM): Feed Generation
Video Search:

text

YouTube Search: "ByteByteGo news feed system design"
OR "Gaurav Sen Twitter timeline architecture"

Duration: 45-50 minutes
Creator: ByteByteGo or Gaurav Sen

Topics:
- PUSH strategy: Add tweet to followers' feeds
- PULL strategy: Fetch tweets on demand
- HYBRID: Best of both
- At scale: Famous users can't use PUSH (too many followers)
Coding Task: Feed Algorithm

Create file: Week2/FeedService.cs

csharp

using StackExchange.Redis;

public class FeedService
{
    private readonly IDistributedCache _cache;
    private readonly AppDbContext _db;
    private const int FEED_CACHE_SIZE = 1000;
    private const string FEED_KEY_PREFIX = "feed:";
    
    public FeedService(IDistributedCache cache, AppDbContext db)
    {
        _cache = cache;
        _db = db;
    }
    
    // When someone posts: use PUSH or PULL based on followers
    public async Task OnTweetCreatedAsync(Tweet tweet)
    {
        var followerCount = await _db.GetFollowerCountAsync(tweet.UserId);
        
        // HYBRID: Push for normal users, Pull for celebrities
        if (followerCount < 1_000_000)
        {
            // PUSH: Add to all followers' feeds
            await PushToFollowerFeedsAsync(tweet);
        }
        else
        {
            // PULL: Celebrities' followers will fetch on demand
            await _cache.SetAsync(
                $"celebrity_feed:{tweet.UserId}",
                tweet,
                TimeSpan.FromDays(7)
            );
        }
    }
    
    // PUSH: Add tweet to all followers' cached feeds
    private async Task PushToFollowerFeedsAsync(Tweet tweet)
    {
        var followerIds = await _db.GetFollowerIdsAsync(tweet.UserId);
        
        foreach (var followerId in followerIds)
        {
            var feedKey = $"{FEED_KEY_PREFIX}{followerId}";
            var cachedFeed = await _cache.GetAsync<List<Tweet>>(feedKey) 
                ?? new List<Tweet>();
            
            // Add new tweet at top
            cachedFeed.Insert(0, tweet);
            
            // Keep only top FEED_CACHE_SIZE tweets
            if (cachedFeed.Count > FEED_CACHE_SIZE)
                cachedFeed.RemoveAt(FEED_CACHE_SIZE - 1);
            
            // Update cache
            await _cache.SetAsync(
                feedKey,
                cachedFeed,
                TimeSpan.FromDays(7)
            );
        }
    }
    
    // Get user's feed (mix of PUSH and PULL)
    public async Task<List<Tweet>> GetUserFeedAsync(long userId)
    {
        var feedKey = $"{FEED_KEY_PREFIX}{userId}";
        var cachedFeed = await _cache.GetAsync<List<Tweet>>(feedKey);
        
        if (cachedFeed != null)
            return cachedFeed;  // Cached, return immediately
        
        // Cache miss: Rebuild feed (PULL strategy)
        var followingIds = await _db.GetFollowingIdsAsync(userId);
        var tweets = new List<Tweet>();
        
        foreach (var followingId in followingIds)
        {
            // Get tweets from this user
            var userTweets = await _db.Tweets
                .Where(t => t.UserId == followingId && !t.IsDeleted)
                .OrderByDescending(t => t.CreatedAt)
                .Take(50)
                .ToListAsync();
            tweets.AddRange(userTweets);
        }
        
        // Sort by date (newest first)
        tweets = tweets
            .OrderByDescending(t => t.CreatedAt)
            .Take(100)
            .ToList();
        
        // Cache the feed
        await _cache.SetAsync(
            feedKey,
            tweets,
            TimeSpan.FromMinutes(10)
        );
        
        return tweets;
    }
}
Checklist:

 Understand PUSH vs PULL vs HYBRID
 Implement feed generation
 Cache management working
Git Commit:

Bash

git add .
git commit -m "Week2-Day3-Morning: Hybrid feed generation algorithm"
git push origin main
EVENING (9-10 PM): Cache Invalidation
Video Search:

text

YouTube Search: "Cache invalidation strategies"
OR "Redis cache invalidation patterns"

Duration: 20-25 minutes

Topics:
- Problem: User unfollows but old tweets still in cache
- Solution: Event-based invalidation
- Delete from cache immediately on changes
Coding Task: Cache Invalidation

Add to Week2/FeedService.cs:

csharp

// When user unfollows someone
public async Task OnUserUnfollowsAsync(long userId, long unfollowingId)
{
    // Remove tweets from cache immediately
    var feed = await _cache.GetAsync<List<Tweet>>(
        $"{FEED_KEY_PREFIX}{userId}"
    );
    
    if (feed != null)
    {
        feed = feed
            .Where(t => t.UserId != unfollowingId)
            .ToList();
        
        await _cache.SetAsync(
            $"{FEED_KEY_PREFIX}{userId}",
            feed
        );
    }
}

// When tweet is deleted
public async Task OnTweetDeletedAsync(Tweet tweet)
{
    var followers = await _db.GetFollowerIdsAsync(tweet.UserId);
    
    foreach (var followerId in followers)
    {
        var feedKey = $"{FEED_KEY_PREFIX}{followerId}";
        // Delete feed cache so it gets rebuilt on next request
        await _cache.RemoveAsync(feedKey);
    }
}
Checklist:

 Cache invalidation working
 Immediate consistency
 No stale data in feeds
Git Commit:

Bash

git add .
git commit -m "Week2-Day3-Evening: Cache invalidation on feed changes"
git push origin main
THURSDAY, OCT 12
MORNING (8-10 AM): Caching Strategies
Video Search:

text

YouTube Search: "ByteByteGo caching strategies"
OR "Milan Jovanovic Redis caching ASP.NET Core"

Duration: 40-50 minutes
Creator: ByteByteGo or Milan Jovanović

Topics:
- L1: Browser cache
- L2: CDN cache
- L3: Application cache (Redis)
- L4: Database cache
- Strategy: Cache-Aside, Write-Through, Write-Behind
Coding Task: Multi-Layer Cache

Create file: Week2/MultiLayerCache.cs

csharp

public class MultiLayerCacheService
{
    private readonly IDistributedCache _redisCache;
    private readonly IMemoryCache _localCache;
    
    public async Task<Tweet> GetTweetAsync(long tweetId)
    {
        // L1: Check local memory cache (< 1ms)
        if (_localCache.TryGetValue($"tweet:{tweetId}", out Tweet cachedTweet))
        {
            return cachedTweet;
        }
        
        // L2: Check Redis cache (1-5ms over network)
        var redisCached = await _redisCache.GetStringAsync($"tweet:{tweetId}");
        if (redisCached != null)
        {
            var tweet = JsonSerializer.Deserialize<Tweet>(redisCached);
            
            // Populate local cache
            _localCache.Set(
                $"tweet:{tweetId}",
                tweet,
                TimeSpan.FromMinutes(5)
            );
            
            return tweet;
        }
        
        // L3: Database (100-200ms)
        var dbTweet = await _db.Tweets.FindAsync(tweetId);
        
        if (dbTweet != null)
        {
            // Populate all caches
            await _redisCache.SetStringAsync(
                $"tweet:{tweetId}",
                JsonSerializer.Serialize(dbTweet),
                new DistributedCacheEntryOptions
                {
                    AbsoluteExpirationRelativeToNow = TimeSpan.FromDays(1)
                }
            );
            
            _localCache.Set(
                $"tweet:{tweetId}",
                dbTweet,
                TimeSpan.FromMinutes(5)
            );
        }
        
        return dbTweet;
    }
}
Checklist:

 Multi-layer cache working
 Response time < 1ms with L1 hit
 Fallback to L2 then L3
Git Commit:

Bash

git add .
git commit -m "Week2-Day4-Morning: Multi-layer caching implementation"
git push origin main
EVENING (9-10 PM): Cache Stampede
Video Search:

text

YouTube Search: "Cache stampede mitigation"
OR "Distributed lock cache miss"

Duration: 20-25 minutes

Topics:
- Problem: All users hit DB when cache expires
- Solution: Distributed lock OR early refresh
- Prevent thundering herd
Coding Task: Prevent Cache Stampede

Add to Week2/MultiLayerCache.cs:

csharp

public async Task<Tweet> GetTweetWithStampedeProtectionAsync(long tweetId)
{
    var cached = await _redisCache.GetStringAsync($"tweet:{tweetId}");
    
    if (cached != null)
    {
        // Check if approaching expiration
        var expiryTime = await _redisCache.GetExpiryAsync($"tweet:{tweetId}");
        var secondsUntilExpiry = (expiryTime - DateTime.UtcNow).TotalSeconds;
        
        // Refresh early if less than 10% time left
        if (secondsUntilExpiry < 360)  // 10% of 1 hour
        {
            _ = RefreshCacheAsync(tweetId);  // Fire and forget
        }
        
        return JsonSerializer.Deserialize<Tweet>(cached);
    }
    
    // Try to acquire lock
    var lockKey = $"tweet:{tweetId}:lock";
    var lockValue = Guid.NewGuid().ToString();
    
    var lockAcquired = await _redisCache.SetStringAsync(
        lockKey,
        lockValue,
        new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromSeconds(5)
        },
        when: When.NotExists
    );
    
    if (lockAcquired)
    {
        try
        {
            // Only this request fetches from DB
            var tweet = await _db.Tweets.FindAsync(tweetId);
            await _redisCache.SetStringAsync(
                $"tweet:{tweetId}",
                JsonSerializer.Serialize(tweet),
                new DistributedCacheEntryOptions
                {
                    AbsoluteExpirationRelativeToNow = TimeSpan.FromHours(1)
                }
            );
            return tweet;
        }
        finally
        {
            await _redisCache.RemoveAsync(lockKey);
        }
    }
    else
    {
        // Wait and retry
        await Task.Delay(100);
        return await GetTweetWithStampedeProtectionAsync(tweetId);
    }
}

private async Task RefreshCacheAsync(long tweetId)
{
    var tweet = await _db.Tweets.FindAsync(tweetId);
    if (tweet != null)
    {
        await _redisCache.SetStringAsync(
            $"tweet:{tweetId}",
            JsonSerializer.Serialize(tweet),
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromHours(1)
            }
        );
    }
}
Checklist:

 Cache stampede prevention working
 Distributed lock implemented
 Early refresh strategy working
Git Commit:

Bash

git add .
git commit -m "Week2-Day4-Evening: Cache stampede prevention with distributed lock"
git push origin main
FRIDAY, OCT 13
MORNING (8-10 AM): Review & Quiz
Self-Assessment:

text

Q1: What's the difference between PUSH and PULL feed strategy?
A: ___________________________________________

Q2: How do message queues handle 1M writes per second?
A: ___________________________________________

Q3: What is cache stampede and how to prevent it?
A: ___________________________________________

Q4: What's a distributed transaction and why is it hard?
A: ___________________________________________

Q5: Explain multi-layer caching (L1, L2, L3)
A: ___________________________________________

Score: ___/5 correct
MORNING (8-10 AM): Performance Testing
Coding Task: Load Testing

csharp

// Create simple load test to verify queue handling

using (var client = new HttpClient())
{
    var tasks = new List<Task>();
    
    // Simulate 100 concurrent requests
    for (int i = 0; i < 100; i++)
    {
        var task = Task.Run(async () =>
        {
            var request = new { content = "Test tweet from load test" };
            var json = JsonSerializer.Serialize(request);
            var content = new StringContent(json, Encoding.UTF8, "application/json");
            
            var response = await client.PostAsync(
                "http://localhost:5000/api/tweets",
                content
            );
            
            Console.WriteLine($"Response: {response.StatusCode}");
        });
        
        tasks.Add(task);
    }
    
    await Task.WhenAll(tasks);
    Console.WriteLine("Load test complete!");
}
Results:

All requests should return 202 Accepted
Response time < 100ms per request
Background worker processes them
EVENING (9-10 PM): Blog Post
Write: "Handling 1M Tweets Per Second"

Sections:

Problem (100 words) - Concurrent writes crash database
Solution: Message Queues (300 words) - Decouple, async processing
Feed Generation (300 words) - PUSH vs PULL vs HYBRID
Caching Strategy (250 words) - Multi-layer caching
Cache Invalidation (200 words) - Keeping data fresh
Real Numbers (150 words) - Performance metrics
Code Examples (300 words) - Working implementations
Publish on: Medium or Dev.to

Git Commit:

Bash

git add .
git commit -m "Week2 Complete: Message queues, caching, and feed generation"
git push origin main
Week 2 Summary:

text

✅ WEEK 2 COMPLETE!

What You Built:
✓ Message queue system (Azure Service Bus)
✓ Background worker processing
✓ Database transactions
✓ Hybrid feed algorithm
✓ Multi-layer caching
✓ Cache stampede prevention
✓ Load testing

Metrics:
- Hours: 15/15 ✓
- Commits: 14+
- Blog posts: 2/2 ✓

Skills Improved:
- .NET: 3.5/5 → 4/5
- System Design: Starting → 2/5
- Performance: 0/5 → 2.5/5

Next Week: Real-Time Features with WebSockets!
WEEK 3: REAL-TIME FEATURES
Oct 16-22, 2026 | Goal: Live notifications for 10K concurrent users
MONDAY, OCT 16
MORNING (8-10 AM): WebSocket Architecture
Video Search:

text

YouTube Search: "Hussein Nasser WebSockets explained deep dive"

Duration: 40-50 minutes
Creator: Hussein Nasser

Topics:
- HTTP upgrade handshake
- Persistent full-duplex TCP connection
- Frame-based communication
- When to use vs REST APIs
Coding Task: SignalR Hub

Create file: Week3/NotificationHub.cs

csharp

using Microsoft.AspNetCore.SignalR;

public class NotificationHub : Hub
{
    private readonly ILogger<NotificationHub> _logger;
    
    public NotificationHub(ILogger<NotificationHub> logger)
    {
        _logger = logger;
    }
    
    public override async Task OnConnectedAsync()
    {
        var userId = Context.User?.FindFirst("UserId")?.Value;
        _logger.LogInformation("User {UserId} connected. ConnectionId: {ConnectionId}", 
            userId, Context.ConnectionId);
        
        await base.OnConnectedAsync();
    }
    
    public override async Task OnDisconnectedAsync(Exception exception)
    {
        _logger.LogInformation("User disconnected. ConnectionId: {ConnectionId}", 
            Context.ConnectionId);
        
        await base.OnDisconnectedAsync(exception);
    }
    
    // Client can call this to send message
    public async Task SendNotification(string message)
    {
        var userId = Context.User?.FindFirst("UserId")?.Value;
        
        // Broadcast to all connected clients
        await Clients.All.SendAsync("ReceiveNotification", 
            new { userId, message, timestamp = DateTime.UtcNow });
    }
}
Configure in Startup:

csharp

// In ConfigureServices
services.AddSignalR();

// In Configure
app.MapHub<NotificationHub>("/hubs/notifications");
Checklist:

 SignalR hub created
 Can connect from client
 Messages broadcast to all
Git Commit:

Bash

git add .
git commit -m "Week3-Day1-Morning: SignalR notification hub setup"
git push origin main
EVENING (9-10 PM): Real-Time Scalability
Video Search:

text

YouTube Search: "Hussein Nasser scaling WebSockets Redis backplane"

Duration: 30-35 minutes
Creator: Hussein Nasser

Topics:
- Single server: 10K connections
- Multiple servers: How to broadcast to all?
- Redis backplane: Message distribution
- Horizontal scaling WebSockets
Coding Task: Redis Backplane

Add to Startup:

csharp

// Install NuGet
// Install-Package Microsoft.AspNetCore.SignalR.StackExchangeRedis

services.AddSignalR()
    .AddStackExchangeRedis(options =>
    {
        options.ConnectionFactory = async writer =>
        {
            var connection = await ConnectionMultiplexer.ConnectAsync("localhost:6379");
            return connection;
        };
    });
Now SignalR can broadcast across multiple servers via Redis!

Checklist:

 Redis backplane configured
 Can run multiple servers
 Messages reach all clients
Git Commit:

Bash

git add .
git commit -m "Week3-Day1-Evening: Redis backplane for multi-server scaling"
git push origin main
TUESDAY, OCT 17
MORNING (8-10 AM): Real-Time Tweet Delivery
Video Search:

text

YouTube Search: "Real-time notifications architecture system design"

Duration: 40-50 minutes

Topics:
- When tweet created: notify followers
- Real-time feed updates
- Push notifications to connected clients
Coding Task: Live Tweet Notifications

Create file: Week3/RealTimeTweetService.cs

csharp

using Microsoft.AspNetCore.SignalR;

public class RealTimeTweetService
{
    private readonly IHubContext<NotificationHub> _hubContext;
    private readonly AppDbContext _db;
    
    public RealTimeTweetService(
        IHubContext<NotificationHub> hubContext,
        AppDbContext db)
    {
        _hubContext = hubContext;
        _db = db;
    }
    
    // When new tweet created: notify followers in real-time
    public async Task NotifyFollowersOfNewTweetAsync(Tweet tweet)
    {
        // Get list of followers
        var followerIds = await _db.GetFollowerIdsAsync(tweet.UserId);
        
        // Prepare notification
        var notification = new
        {
            tweetId = tweet.TweetId,
            userId = tweet.UserId,
            content = tweet.Content,
            createdAt = tweet.CreatedAt
        };
        
        // Send to each follower
        foreach (var followerId in followerIds)
        {
            await _hubContext.Clients
                .User(followerId.ToString())
                .SendAsync("NewTweet", notification);
        }
    }
}
Modify TweetProcessingWorker:

csharp

// In TweetProcessingWorker.ProcessMessageAsync
await _db.SaveChangesAsync();

// NEW: Notify followers in real-time
await _realTimeTweetService.NotifyFollowersOfNewTweetAsync(tweet);

await args.CompleteMessageAsync(message);
Checklist:

 Real-time notifications working
 Followers receive tweets immediately
 Multiple clients can connect
Git Commit:

Bash

git add .
git commit -m "Week3-Day2-Morning: Real-time tweet notifications via SignalR"
git push origin main
EVENING (9-10 PM): Presence Tracking
Video Search:

text

YouTube Search: "System design online offline status presence service"

Duration: 35-40 minutes

Topics:
- Who's online right now?
- Typing indicators
- Presence updates
- Scale to 10M users
Coding Task: User Presence

Add to NotificationHub:

csharp

public class NotificationHub : Hub
{
    private readonly IDistributedCache _cache;
    
    public override async Task OnConnectedAsync()
    {
        var userId = Context.User?.FindFirst("UserId")?.Value;
        
        // Mark user as online
        await _cache.SetStringAsync(
            $"user:online:{userId}",
            DateTime.UtcNow.ToString(),
            TimeSpan.FromHours(1)
        );
        
        // Notify all clients
        await Clients.All.SendAsync("UserOnline", userId);
        
        await base.OnConnectedAsync();
    }
    
    public override async Task OnDisconnectedAsync(Exception exception)
    {
        var userId = Context.User?.FindFirst("UserId")?.Value;
        
        // Mark user as offline
        await _cache.RemoveAsync($"user:online:{userId}");
        
        // Notify all clients
        await Clients.All.SendAsync("UserOffline", userId);
        
        await base.OnDisconnectedAsync(exception);
    }
    
    // Send typing indicator
    public async Task SendTyping(string recipientId)
    {
        await Clients.User(recipientId)
            .SendAsync("UserTyping", Context.User.FindFirst("UserId")?.Value);
    }
}
Checklist:

 Online status tracked in Redis
 Presence updates broadcast
 Typing indicators working
Git Commit:

Bash

git add .
git commit -m "Week3-Day2-Evening: User presence tracking with online/offline status"
git push origin main
WEDNESDAY, OCT 18
MORNING (8-10 AM): WebSocket vs SSE
Video Search:

text

YouTube Search: "Hussein Nasser WebSockets vs Server Sent Events"

Duration: 35-40 minutes
Creator: Hussein Nasser

Topics:
- WebSockets: Full duplex (client ↔ server)
- SSE: One-way server → client
- When to use which
- Browser compatibility
Coding Task: SSE Alternative

Create file: Week3/ServerSentEventsController.cs

csharp

[ApiController]
[Route("api/[controller]")]
public class EventsController : ControllerBase
{
    private readonly IDistributedCache _cache;
    
    [HttpGet("stream/{userId}")]
    public async IAsyncEnumerable<string> StreamNotifications(
        long userId,
        CancellationToken cancellationToken)
    {
        while (!cancellationToken.IsCancellationRequested)
        {
            // Check for new notifications
            var notifications = await _cache.GetStringAsync($"notifications:{userId}");
            
            if (notifications != null)
            {
                yield return $"data: {notifications}\n\n";
                await _cache.RemoveAsync($"notifications:{userId}");
            }
            
            // Send heartbeat
            yield return ": heartbeat\n\n";
            
            // Wait before checking again
            await Task.Delay(1000, cancellationToken);
        }
    }
}
Client-side (HTML):

HTML

<script>
// WebSocket option (full-duplex)
const hubConnection = new signalR.HubConnectionBuilder()
    .withUrl("/hubs/notifications")
    .withAutomaticReconnect()
    .build();

hubConnection.on("NewTweet", (notification) => {
    console.log("New tweet:", notification);
});

hubConnection.start();

// OR SSE option (one-way)
const eventSource = new EventSource("/api/events/stream/123");
eventSource.onmessage = (event) => {
    console.log("Notification:", event.data);
};
</script>
Checklist:

 Both WebSocket and SSE working
 Understand trade-offs
 Know when to use each
Git Commit:

Bash

git add .
git commit -m "Week3-Day3-Morning: SSE alternative to WebSockets"
git push origin main
EVENING (9-10 PM): Connection Management
Video Search:

text

YouTube Search: "WebSocket keep alive ping pong heartbeat"

Duration: 20-25 minutes

Topics:
- Detecting dropped connections
- Automatic reconnection
- Heartbeat pings
- Connection pooling
Coding Task: Connection Stability

Add to Startup:

csharp

services.AddSignalR(options =>
{
    // Client timeout (30 seconds)
    options.ClientTimeoutInterval = TimeSpan.FromSeconds(30);
    
    // Keep-alive interval (15 seconds)
    options.KeepAliveInterval = TimeSpan.FromSeconds(15);
    
    // Max buffer size
    options.MaximumReceiveMessageSize = 32 * 1024; // 32 KB
});
Client-side reconnection:

csharp

const hubConnection = new signalR.HubConnectionBuilder()
    .withUrl("/hubs/notifications")
    .withAutomaticReconnect([
        0,      // Immediately retry
        2000,   // After 2 seconds
        5000,   // After 5 seconds
        10000,  // After 10 seconds
        30000   // After 30 seconds
    ])
    .withServerTimeout(15000)  // 15 second timeout
    .build();

hubConnection.onreconnecting((error) => {
    console.log("Reconnecting...", error);
});

hubConnection.onreconnected((connectionId) => {
    console.log("Reconnected!");
});
Checklist:

 Keep-alive working
 Auto-reconnection on disconnect
 Stable connection maintained
Git Commit:

Bash

git add .
git commit -m "Week3-Day3-Evening: Connection stability with keep-alive and reconnection"
git push origin main
THURSDAY, OCT 19
MORNING (8-10 AM): Load Testing Real-Time
Video Search:

text

YouTube Search: "k6 load testing WebSockets concurrent connections"

Duration: 35-40 minutes

Topics:
- Virtual users
- Concurrent connections
- Message throughput
- Stress testing WebSocket servers
Coding Task: Load Test Script

Create file: Week3/load-test.js (k6 script):

JavaScript

import ws from 'k6/ws';
import { check } from 'k6';

export const options = {
    vus: 100,           // 100 virtual users
    duration: '30s',    // 30 seconds
    thresholds: {
        'ws_connecting': ['p(95)<500'],  // 95% connect in <500ms
        'ws_session_duration': ['avg<5000'], // avg session <5s
    }
};

export default function() {
    const url = 'ws://localhost:5000/hubs/notifications';
    const params = {
        tags: { name: 'RealTimeTest' }
    };
    
    let response = ws.connect(url, params, function(socket) {
        socket.on('open', () => {
            console.log('Connected');
            
            // Send message every 1 second
            socket.setInterval(() => {
                socket.send(JSON.stringify({ 
                    method: 'SendNotification',
                    args: ['Test message']
                }));
            }, 1000);
        });
        
        socket.on('message', (data) => {
            check(data, {
                'received message': (msg) => msg.length > 0
            });
        });
        
        socket.on('close', () => {
            console.log('Disconnected');
        });
        
        socket.setTimeout(() => {
            socket.close();
        }, 29000);  // Close after 29 seconds
    });
    
    check(response, {
        'status is 101': (r) => r && r.status === 101
    });
}
Run test:

Bash

k6 run load-test.js
Expected results:

100 concurrent WebSocket connections
< 500ms to connect
Messages received in real-time
Checklist:

 k6 installed
 Load test script working
 Server handles 100+ concurrent connections
Git Commit:

Bash

git add .
git commit -m "Week3-Day4-Morning: Load testing WebSocket connections with k6"
git push origin main
EVENING (9-10 PM): Architecture Documentation
Coding Task: Document Real-Time Architecture

Create file: Week3/REAL-TIME-ARCHITECTURE.md

Markdown

# Real-Time Notification Architecture

## Overview
TechTweet uses SignalR with Redis backplane for real-time updates.

## Components

### 1. SignalR Hub (NotificationHub)
- Handles WebSocket connections
- Broadcasts messages to clients
- Tracks user presence

### 2. Redis Backplane
- Distributes messages across servers
- Enables horizontal scaling
- Pub/Sub for real-time events

### 3. Background Worker
- Listens to tweet creation events
- Sends real-time notifications
- Updates live feeds

## Flow
Tweet Created
↓
Message to Queue
↓
Background Worker
↓
NotifyFollowersOfNewTweetAsync()
↓
SignalR Broadcast to Followers
↓
Clients Receive in Real-Time

text


## Scaling

- Single Server: 10K concurrent connections
- Multiple Servers: Via Redis backplane
- Max Connections: Limited by memory/network

## Performance

- Connection Setup: < 500ms
- Message Delivery: < 100ms
- Throughput: 10K+ messages/sec

## Monitoring

- Track active connections
- Monitor message latency
- Alert on disconnections
Checklist:

 Architecture documented
 Flow diagrams clear
 Performance metrics included
Git Commit:

Bash

git add .
git commit -m "Week3-Day4-Evening: Real-time architecture documentation"
git push origin main
FRIDAY, OCT 20
MORNING (8-10 AM): Review & Testing
Test Checklist:

text

✓ WebSocket connections work
✓ Real-time tweet notifications deliver
✓ User presence tracking updates
✓ Typing indicators send
✓ Connection stable with keep-alive
✓ Auto-reconnect on disconnect
✓ Load test: 100 concurrent users
✓ SSE alternative works
EVENING (9-10 PM): Blog Post
Write: "Real-Time Features at Scale"

Sections:

Problem (100 words) - Why real-time matters
WebSocket Architecture (250 words) - How it works
Scaling Strategy (250 words) - Redis backplane, multi-server
Presence & Typing (200 words) - User activity tracking
Load Testing (150 words) - 10K concurrent users
Code Examples (300 words) - Implementation
Publish on: Medium or Dev.to

Git Commit:

Bash

git add .
git commit -m "Week3 Complete: Real-time WebSocket notifications at scale"
git push origin main
Week 3 Summary:

text

✅ WEEK 3 COMPLETE!

What You Built:
✓ SignalR hub for real-time
✓ Redis backplane for scaling
✓ Real-time tweet notifications
✓ User presence tracking
✓ Typing indicators
✓ SSE alternative
✓ Connection stability
✓ Load testing

Metrics:
- Hours: 15/15 ✓
- Commits: 14+
- Blog posts: 3/3 ✓

Skills Improved:
- Real-Time: 0/5 → 3/5
- System Design: 2/5 → 2.5/5

Next Week: API Gateway & Rate Limiting!
WEEK 4: API GATEWAY & RATE LIMITING
Oct 23-29, 2026 | Goal: Protect API from abuse, route to services
MONDAY, OCT 23
MORNING (8-10 AM): API Gateway Patterns
Video Search:

text

YouTube Search: "Hussein Nasser reverse proxy vs API gateway"
OR "ByteByteGo API gateway architecture"

Duration: 40-50 minutes
Creator: Hussein Nasser or ByteByteGo

Topics:
- What is gateway
- Routing between services
- SSL termination
- Cross-cutting concerns
Coding Task: Basic Gateway with YARP

Create file: Week4/GatewayStartup.cs

csharp

// Install NuGet
// Install-Package Yarp.ReverseProxy

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddReverseProxy()
    .LoadFromConfig(builder.Configuration.GetSection("ReverseProxy"));

var app = builder.Build();

app.MapReverseProxy();

app.Run();
Configuration in appsettings.json:

JSON

{
  "ReverseProxy": {
    "Routes": {
      "auth": {
        "ClusterId": "auth-cluster",
        "Match": {
          "Path": "/api/auth/{**catch-all}"
        }
      },
      "tweets": {
        "ClusterId": "tweets-cluster",
        "Match": {
          "Path": "/api/tweets/{**catch-all}"
        }
      },
      "feed": {
        "ClusterId": "feed-cluster",
        "Match": {
          "Path": "/api/feed/{**catch-all}"
        }
      }
    },
    "Clusters": {
      "auth-cluster": {
        "Destinations": {
          "auth-service": {
            "Address": "http://localhost:5001"
          }
        }
      },
      "tweets-cluster": {
        "Destinations": {
          "tweets-service": {
            "Address": "http://localhost:5002"
          }
        }
      },
      "feed-cluster": {
        "Destinations": {
          "feed-service": {
            "Address": "http://localhost:5003"
          }
        }
      }
    }
  }
}
Checklist:

 YARP installed
 Routes configured
 Can route to backend services
Git Commit:

Bash

git add .
git commit -m "Week4-Day1-Morning: API Gateway with YARP routing"
git push origin main
EVENING (9-10 PM): Rate Limiting
Video Search:

text

YouTube Search: "ByteByteGo rate limiting algorithms token bucket"

Duration: 35-40 minutes
Creator: ByteByteGo

Topics:
- Token Bucket algorithm
- Leaky Bucket
- Sliding Window
- When to use each
Coding Task: Rate Limiting

Create file: Week4/RateLimitingMiddleware.cs

csharp

// Install NuGet
// Install-Package Microsoft.AspNetCore.RateLimiting

var builder = WebApplication.CreateBuilder(args);

// Add rate limiting
builder.Services.AddRateLimiter(options =>
{
    options.GlobalLimiter = PartitionedRateLimiter.Create<HttpContext, string>(context =>
        RateLimitPartition.GetTokenBucketLimiter(
            partitionKey: context.User.FindFirst("UserId")?.Value ?? context.Connection.RemoteIpAddress?.ToString() ?? "anonymous",
            factory: partition => new TokenBucketRateLimiterOptions
            {
                TokenLimit = 100,           // 100 requests
                QueueProcessingOrder = QueueProcessingOrder.OldestFirst,
                QueueLimit = 0,             // No queue
                ReplenishmentPeriod = TimeSpan.FromMinutes(1),  // Per minute
                TokensPerPeriod = 100       // 100 tokens per minute
            }
        ));

    options.OnRejected = (context, cancellationToken) =>
    {
        context.HttpContext.Response.StatusCode = StatusCodes.Status429TooManyRequests;
        context.HttpContext.Response.WriteAsync("Rate limit exceeded");
        return ValueTask.CompletedTask;
    };
});

var app = builder.Build();

app.UseRateLimiter();

app.Run();
Per-endpoint limits:

csharp

[HttpPost("tweets")]
[RateLimiter("tweet-limiter")]
public async Task<IActionResult> CreateTweet([FromBody] CreateTweetRequest request)
{
    // ...
}
Configure specific limiter:

csharp

options.AddPolicy("tweet-limiter", context =>
    RateLimitPartition.GetTokenBucketLimiter(
        partitionKey: context.User.FindFirst("UserId")?.Value ?? "anonymous",
        factory: partition => new TokenBucketRateLimiterOptions
        {
            TokenLimit = 10,  // 10 tweets per
            ReplenishmentPeriod = TimeSpan.FromHours(1),  // 1 hour
            TokensPerPeriod = 10
        }));
Checklist:

 Rate limiting configured globally
 Per-endpoint limits working
 429 status returned when exceeded
Git Commit:

Bash

git add .
git commit -m "Week4-Day1-Evening: Token bucket rate limiting"
git push origin main
TUESDAY, OCT 24
MORNING (8-10 AM): Circuit Breaker Pattern
Video Search:

text

YouTube Search: "ByteByteGo circuit breaker pattern"
OR "Polly resilience .NET"

Duration: 40-50 minutes
Creator: ByteByteGo or Code Maze

Topics:
- Closed, Open, Half-Open states
- Preventing cascading failures
- Failing fast
- Automatic recovery
Coding Task: Circuit Breaker

Create file: Week4/CircuitBreakerMiddleware.cs

csharp

// Install NuGet
// Install-Package Polly
// Install-Package Polly.CircuitBreaker

using Polly;
using Polly.CircuitBreaker;

var builder = WebApplication.CreateBuilder(args);

// Create circuit breaker policy
var circuitBreakerPolicy = Policy
    .Handle<HttpRequestException>()
    .Or<TaskCanceledException>()
    .OrResult<HttpResponseMessage>(r => !r.IsSuccessStatusCode)
    .CircuitBreaker(
        handledEventsAllowedBeforeBreaking: 5,  // 5 failures
        durationOfBreak: TimeSpan.FromSeconds(30),  // Break for 30 sec
        onBreak: (outcome, duration) =>
        {
            Console.WriteLine($"Circuit breaker opened for {duration.TotalSeconds}s");
        },
        onReset: () =>
        {
            Console.WriteLine("Circuit breaker reset");
        }
    );

// Create retry policy
var retryPolicy = Policy
    .Handle<HttpRequestException>()
    .Or<TaskCanceledException>()
    .OrResult<HttpResponseMessage>(r => !r.IsSuccessStatusCode)
    .WaitAndRetry(
        retryCount: 3,
        sleepDurationProvider: attempt =>
            TimeSpan.FromSeconds(Math.Pow(2, attempt)),  // Exponential backoff
        onRetry: (outcome, duration, retryCount, context) =>
        {
            Console.WriteLine($"Retry {retryCount} after {duration.TotalSeconds}s");
        }
    );

// Combine policies (retry then circuit breaker)
var policy = Policy.Wrap(retryPolicy, circuitBreakerPolicy);

// Use in HTTP client
services.AddHttpClient("ResilientClient")
    .AddPolicyHandler(policy);

var app = builder.Build();

// Use in service
public class TweetsService
{
    private readonly HttpClient _httpClient;
    
    public TweetsService(IHttpClientFactory httpClientFactory)
    {
        _httpClient = httpClientFactory.CreateClient("ResilientClient");
    }
    
    public async Task<Tweet> GetTweetAsync(long tweetId)
    {
        try
        {
            var response = await _httpClient.GetAsync($"http://tweets-service/tweets/{tweetId}");
            if (response.IsSuccessStatusCode)
            {
                var json = await response.Content.ReadAsStringAsync();
                return JsonSerializer.Deserialize<Tweet>(json);
            }
        }
        catch (BrokenCircuitException)
        {
            // Circuit breaker is open, fail fast
            throw new ServiceUnavailableException("Tweets service unavailable");
        }
        
        return null;
    }
}
Checklist:

 Circuit breaker opens after failures
 Fails fast when open
 Automatically resets
 Retry with backoff working
Git Commit:

Bash

git add .
git commit -m "Week4-Day2-Morning: Circuit breaker with Polly resilience"
git push origin main
EVENING (9-10 PM): Distributed Tracing
Video Search:

text

YouTube Search: "OpenTelemetry distributed tracing explained"

Duration: 35-40 minutes

Topics:
- Trace IDs
- Span IDs
- Context propagation
- Debugging across services
Coding Task: Add Tracing

csharp

// Install NuGet
// Install-Package OpenTelemetry
// Install-Package OpenTelemetry.Exporter.Console

var builder = WebApplication.CreateBuilder(args);

// Add OpenTelemetry
builder.Services.AddOpenTelemetry()
    .WithTracing(tracing =>
    {
        tracing
            .AddAspNetCoreInstrumentation()
            .AddHttpClientInstrumentation()
            .AddSqlClientInstrumentation()
            .AddConsoleExporter();
    });

var app = builder.Build();

app.Run();
Each request now has:

Trace ID (unique per request)
Span IDs (per service)
Context propagated across services
Checklist:

 OpenTelemetry configured
 Traces being exported
 Can trace across services
Git Commit:

Bash

git add .
git commit -m "Week4-Day2-Evening: OpenTelemetry distributed tracing"
git push origin main
WEDNESDAY, OCT 25
MORNING (8-10 AM): Health Checks
Video Search:

text

YouTube Search: "ASP.NET Core health checks tutorial"

Duration: 30-35 minutes

Topics:
- Liveness probes
- Readiness probes
- Load balancer integration
- Graceful shutdown
Coding Task: Health Checks

csharp

builder.Services.AddHealthChecks()
    .AddCheck("database", async () =>
    {
        try
        {
            await _db.Database.ExecuteSqlRawAsync("SELECT 1");
            return HealthCheckResult.Healthy();
        }
        catch
        {
            return HealthCheckResult.Unhealthy("Database unavailable");
        }
    })
    .AddCheck("redis", async () =>
    {
        try
        {
            var redis = await ConnectionMultiplexer.ConnectAsync("localhost:6379");
            await redis.GetDatabase().PingAsync();
            return HealthCheckResult.Healthy();
        }
        catch
        {
            return HealthCheckResult.Unhealthy("Redis unavailable");
        }
    });

app.MapHealthChecks("/health/live");  // Liveness probe
app.MapHealthChecks("/health/ready"); // Readiness probe

app.Run();
Checklist:

 Health checks configured
 Database check working
 Redis check working
 Can hit /health/live and /health/ready
Git Commit:

Bash

git add .
git commit -m "Week4-Day3-Morning: Health checks for load balancer"
git push origin main
EVENING (9-10 PM): Structured Logging
Video Search:

text

YouTube Search: "Serilog structured logging ASP.NET Core"

Duration: 30-35 minutes

Topics:
- Structured log data
- JSON payloads
- Enriched properties
- Centralized logging
Coding Task: Serilog Setup

csharp

// Install NuGet
// Install-Package Serilog.AspNetCore
// Install-Package Serilog.Sinks.Console

var builder = WebApplication.CreateBuilder(args);

// Configure Serilog
builder.Host.UseSerilog((context, loggerConfig) =>
{
    loggerConfig
        .MinimumLevel.Information()
        .WriteTo.Console(new CompactJsonFormatter())
        .Enrich.FromLogContext()
        .Enrich.WithProperty("Application", "TechTweet")
        .Enrich.WithProperty("Environment", context.HostingEnvironment.EnvironmentName);
});

var app = builder.Build();

app.MapPost("/api/tweets", async (
    [FromBody] CreateTweetRequest request,
    ILogger<Program> logger) =>
{
    using (LogContext.PushProperty("OperationId", Guid.NewGuid()))
    {
        logger.LogInformation(
            "Creating tweet with content length {ContentLength}",
            request.Content.Length);
        
        // ... create tweet ...
        
        logger.LogInformation("Tweet created successfully");
    }
});

app.Run();
Example log output:

JSON

{
  "Timestamp": "2024-10-25T10:30:45.123Z",
  "Level": "Information",
  "MessageTemplate": "Creating tweet with content length {ContentLength}",
  "Properties": {
    "ContentLength": 150,
    "OperationId": "abc123",
    "Application": "TechTweet",
    "Environment": "Production"
  }
}
Checklist:

 Serilog configured
 Structured logs in JSON
 Properties enriched
 Can parse logs easily
Git Commit:

Bash

git add .
git commit -m "Week4-Day3-Evening: Structured logging with Serilog"
git push origin main
THURSDAY, OCT 26
MORNING (8-10 AM): Request/Response Logging
Coding Task: Log All Traffic

csharp

// Middleware to log all requests/responses
app.Use(async (context, next) =>
{
    var request = context.Request;
    var response = context.Response;
    
    var logger = context.RequestServices.GetRequiredService<ILogger<Program>>();
    
    logger.LogInformation(
        "Incoming Request: {Method} {Path}",
        request.Method,
        request.Path);
    
    // Capture response
    var originalBodyStream = response.Body;
    using (var responseBody = new MemoryStream())
    {
        response.Body = responseBody;
        
        await next();
        
        logger.LogInformation(
            "Outgoing Response: {StatusCode} took {Ms}ms",
            response.StatusCode,
            context.Items["ElapsedMs"]);
        
        await responseBody.CopyToAsync(originalBodyStream);
    }
});
Checklist:

 All requests logged
 All responses logged
 Performance metrics captured
Git Commit:

Bash

git add .
git commit -m "Week4-Day4-Morning: Request/response logging middleware"
git push origin main
EVENING (9-10 PM): Error Handling
Coding Task: Global Error Handler

csharp

// Global exception handler
app.UseExceptionHandler(errorApp =>
{
    errorApp.Run(async context =>
    {
        var logger = context.RequestServices.GetRequiredService<ILogger<Program>>();
        var exceptionHandlerPathFeature = context.Features.Get<IExceptionHandlerPathFeature>();
        
        var exception = exceptionHandlerPathFeature?.Error;
        
        logger.LogError(exception, "Unhandled exception");
        
        context.Response.StatusCode = StatusCodes.Status500InternalServerError;
        context.Response.ContentType = "application/json";
        
        await context.Response.WriteAsJsonAsync(new
        {
            error = "Internal server error",
            traceId = context.TraceIdentifier
        });
    });
});
Checklist:

 Global error handling working
 All exceptions caught
 Error response formatted
 Trace ID for debugging
Git Commit:

Bash

git add .
git commit -m "Week4-Day4-Evening: Global error handling with logging"
git push origin main
FRIDAY, OCT 27
MORNING (8-10 AM): Review
Checklist:

text

✓ API Gateway routing
✓ Rate limiting (Token Bucket)
✓ Circuit breaker (Polly)
✓ Distributed tracing (OpenTelemetry)
✓ Health checks
✓ Structured logging (Serilog)
✓ Request/response logging
✓ Global error handling
EVENING (9-10 PM): Blog Post
Write: "Building Production-Ready APIs with Gateway & Rate Limiting"

Sections:

API Gateway Architecture (250 words)
Rate Limiting Strategies (250 words)
Circuit Breaker Pattern (200 words)
Observability Stack (250 words)
Error Handling (150 words)
Code Examples (300 words)
Publish on: Medium or Dev.to

Git Commit:

Bash

git add .
git commit -m "Week4 Complete: API Gateway with resilience and observability"
git push origin main
Week 4 Summary:

text

✅ WEEK 4 COMPLETE!

What You Built:
✓ API Gateway with YARP
✓ Rate limiting (100 req/min)
✓ Circuit breaker (Polly)
✓ Distributed tracing (OpenTelemetry)
✓ Health checks
✓ Structured logging (Serilog)
✓ Global error handling

Metrics:
- Hours: 15/15 ✓
- Commits: 12+
- Blog posts: 4/4 ✓

Skills Improved:
- Gateway: 0/5 → 3/5
- Resilience: 0/5 → 3/5
- Observability: 0/5 → 3/5

Next Week: Microservices Architecture!
WEEK 5: MICROSERVICES ARCHITECTURE
Oct 30 - Nov 5, 2026 | Goal: Decompose into independent services
MONDAY, OCT 30
MORNING (8-10 AM): Service Decomposition
Video Search:

text

YouTube Search: "ByteByteGo microservices architecture"
OR "Martin Fowler microservices"

Duration: 45-50 minutes
Creator: ByteByteGo or Martin Fowler

Topics:
- Monolith vs Microservices
- Service boundaries
- Database per service
- Operational challenges
Coding Task: Service Design

Create file: Week5/ServiceDesign.md

Markdown

# TechTweet Microservices Architecture

## Service Decomposition

### 1. User Service
- User registration, authentication
- Profile management
- User preferences

### 2. Tweet Service
- Create, read, update, delete tweets
- Tweet storage and retrieval
- Tweet metadata

### 3. Feed Service
- Generate user feeds
- Follow/unfollow logic
- Timeline aggregation

### 4. Notification Service
- Send notifications
- Store notification history
- Notification preferences

### 5. Search Service
- Index tweets for search
- Full-text search
- Trending topics

## Database per Service
User Service → UserDB
Tweet Service → TweetDB
Feed Service → FeedCache + DB
Notification Service → NotificationDB
Search Service → ElasticSearch

text


## Communication

Between services:
- Async via message queues (RabbitMQ/Service Bus)
- Sync via HTTP/gRPC (only when necessary)
- Event-driven architecture

## Deployment

Each service:
- Runs independently
- Has own Docker image
- Scales independently
- Deployed to Kubernetes
Checklist:

 Service boundaries defined
 Database strategy clear
 Communication patterns documented
Git Commit:

Bash

git add .
git commit -m "Week5-Day1-Morning: Microservices architecture design"
git push origin main
EVENING (9-10 PM): Domain-Driven Design
Video Search:

text

YouTube Search: "Milan Jovanovic Domain Driven Design tactical"

Duration: 40-45 minutes
Creator: Milan Jovanović

Topics:
- Bounded contexts
- Ubiquitous language
- Aggregate roots
- Value objects
Coding Task: Aggregate Design

Create file: Week5/DomainModels.cs

csharp

// Aggregate Root: User
public class User
{
    public long UserId { get; private set; }
    public string Username { get; private set; }
    public Email Email { get; private set; }  // Value Object
    public bool IsActive { get; private set; }
    
    // Invariants (business rules)
    public void Deactivate()
    {
        if (!IsActive)
            throw new InvalidOperationException("User already inactive");
        
        IsActive = false;
    }
}

// Value Object: Email
public class Email
{
    public string Value { get; }
    
    public Email(string value)
    {
        if (!IsValidEmail(value))
            throw new ArgumentException("Invalid email");
        
        Value = value;
    }
    
    private bool IsValidEmail(string email)
    {
        try
        {
            var addr = new System.Net.Mail.MailAddress(email);
            return addr.Address == email;
        }
        catch
        {
            return false;
        }
    }
    
    public override bool Equals(object obj)
    {
        return obj is Email email && Value == email.Value;
    }
}

// Aggregate Root: Tweet
public class Tweet
{
    public long TweetId { get; private set; }
    public long UserId { get; private set; }
    public string Content { get; private set; }
    public DateTime CreatedAt { get; private set; }
    public bool IsDeleted { get; private set; }
    
    // Invariants
    public void Delete()
    {
        if (IsDeleted)
            throw new InvalidOperationException("Tweet already deleted");
        
        IsDeleted = true;
    }
    
    public void UpdateContent(string newContent)
    {
        if (string.IsNullOrEmpty(newContent) || newContent.Length > 280)
            throw new ArgumentException("Invalid content");
        
        Content = newContent;
    }
}

// Domain Event
public class TweetCreatedEvent
{
    public long TweetId { get; set; }
    public long UserId { get; set; }
    public string Content { get; set; }
    public DateTime CreatedAt { get; set; }
}
Checklist:

 Aggregates defined
 Value objects created
 Business rules enforced
 Domain events designed
Git Commit:

Bash

git add .
git commit -m "Week5-Day1-Evening: Domain-driven design with aggregates"
git push origin main
TUESDAY, NOV 1
MORNING (8-10 AM): Bounded Contexts
Video Search:

text

YouTube Search: "Bounded contexts context mapping DDD"

Duration: 35-40 minutes

Topics:
- Bounded context definition
- Context boundaries
- Anti-corruption layers
- Integration patterns
Coding Task: Context Mapping

Create file: Week5/ContextMapping.md

Markdown

# Bounded Contexts

## Context 1: User Context
- Responsibility: User identity and management
- Ubiquitous Language: User, Email, Username, Profile
- Aggregate: User
- Events: UserRegistered, UserProfileUpdated, UserDeactivated

## Context 2: Tweet Context
- Responsibility: Tweet creation and management
- Ubiquitous Language: Tweet, Content, Author, Post, Delete
- Aggregate: Tweet
- Events: TweetCreated, TweetDeleted, TweetUpdated

## Context 3: Feed Context
- Responsibility: Feed generation and consumption
- Ubiquitous Language: Feed, Timeline, Follow, Unfollow, Home Feed
- Aggregate: UserFeed, FollowRelationship
- Events: UserFollowed, UserUnfollowed, FeedGenerated

## Context 4: Notification Context
- Responsibility: Delivering notifications
- Ubiquitous Language: Notification, Delivery, Channel, Preference
- Aggregate: Notification
- Events: NotificationSent, NotificationRead

## Integration Points

### User → Tweet (Anti-Corruption Layer)
User Context
↓
UserDTO (translation)
↓
Tweet Context

text


### Tweet → Feed
TweetCreatedEvent
↓
FeedService listens
↓
Updates Feed

text


### Feed → Notification
UserFollowedEvent
↓
NotificationService listens
↓
Sends notification

Checklist:

 Contexts identified
 Boundaries clear
 Integration patterns designed
Git Commit:

Bash

git add .
git commit -m "Week5-Day2-Morning: Bounded contexts and anti-corruption layers"
git push origin main
EVENING (9-10 PM): Database per Service
Video Search:

text

YouTube Search: "Database per service microservices"

Duration: 30-35 minutes

Topics:
- Polyglot persistence
- Data consistency challenges
- Event sourcing
- CQRS
Coding Task: Service Databases

Create file: Week5/ServiceDatabases.md

Markdown

# Database Strategy

## User Service
Database: SQL Server
- Relational structure for user data
- ACID transactions for consistency
- Normalization

Connection String: Server=userdb; Database=Users

## Tweet Service
Database: SQL Server
- Denormalized for performance
- Fast writes (1M tweets/day)
- Indexes on UserId, CreatedAt

Connection String: Server=tweetdb; Database=Tweets

## Feed Service
Database: Redis
- In-memory feed cache
- Fast read performance
- TTL-based expiration

Connection String: Redis://feedcache:6379

## Search Service
Database: Elasticsearch
- Full-text search index
- Aggregations for trending
- Real-time indexing

Connection String: http://elasticsearch:9200

## Data Synchronization

Event-driven:
Tweet Created
↓
TweetCreatedEvent published
↓
Search Service subscribes
↓
Indexes in Elasticsearch
↓
Feed Service subscribes
↓
Updates Feed Cache

text


No direct queries between services!
Checklist:

 Database strategy per service
 Event-driven sync planned
 No direct DB access between services
Git Commit:

Bash

git add .
git commit -m "Week5-Day2-Evening: Database per service with event sync"
git push origin main
WEDNESDAY, NOV 2
MORNING (8-10 AM): Inter-Service Communication
Video Search:

text

YouTube Search: "Microservices communication REST gRPC"

Duration: 40-45 minutes

Topics:
- Synchronous (HTTP/gRPC)
- Asynchronous (Message queues)
- When to use each
- Error handling
Coding Task: Service-to-Service Calls

Create file: Week5/ServiceCommunication.cs

csharp

// Synchronous: Get user details (short, real-time)
public class UserServiceClient
{
    private readonly HttpClient _httpClient;
    
    public UserServiceClient(IHttpClientFactory httpClientFactory)
    {
        _httpClient = httpClientFactory.CreateClient("UserService");
    }
    
    public async Task<User> GetUserAsync(long userId)
    {
        var response = await _httpClient.GetAsync($"/api/users/{userId}");
        if (!response.IsSuccessStatusCode)
            throw new ServiceUnavailableException("User service unavailable");
        
        var json = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<User>(json);
    }
}

// Asynchronous: Tweet created event (fire and forget)
public class TweetCreatedEventPublisher
{
    private readonly ServiceBusClient _serviceBusClient;
    
    public async Task PublishTweetCreatedAsync(Tweet tweet)
    {
        var sender = _serviceBusClient.CreateSender("tweet.created");
        
        var message = new ServiceBusMessage(JsonSerializer.Serialize(
            new TweetCreatedEvent
            {
                TweetId = tweet.TweetId,
                UserId = tweet.UserId,
                Content = tweet.Content,
                CreatedAt = tweet.CreatedAt
            }
        ));
        
        await sender.SendMessageAsync(message);
        await sender.DisposeAsync();
    }
}

// Message subscriber (Feed Service)
public class TweetCreatedEventHandler
{
    private readonly FeedService _feedService;
    
    public async Task HandleAsync(TweetCreatedEvent @event)
    {
        // Update feed cache
        await _feedService.OnTweetCreatedAsync(@event);
    }
}
Checklist:

 Sync calls via HTTP
 Async via message queues
 Error handling for both
 Circuit breaker on sync calls
Git Commit:

Bash

git add .
git commit -m "Week5-Day3-Morning: Service-to-service communication patterns"
git push origin main
EVENING (9-10 PM): Testing Services in Isolation
Video Search:

text

YouTube Search: "Microservices integration testing mocking"

Duration: 30-35 minutes

Topics:
- Unit testing services
- Mocking external services
- Contract testing
- Integration testing
Coding Task: Service Tests

Create file: Week5/ServiceTests.cs

csharp

using Moq;
using Xunit;

public class TweetServiceTests
{
    private readonly Mock<IUserServiceClient> _userServiceMock;
    private readonly Mock<IMessagePublisher> _messageMock;
    private readonly TweetService _tweetService;
    
    public TweetServiceTests()
    {
        _userServiceMock = new Mock<IUserServiceClient>();
        _messageMock = new Mock<IMessagePublisher>();
        _tweetService = new TweetService(_userServiceMock.Object, _messageMock.Object);
    }
    
    [Fact]
    public async Task CreateTweet_ValidInput_PublishesEvent()
    {
        // Arrange
        var userId = 123;
        var content = "Test tweet";
        
        // Act
        await _tweetService.CreateTweetAsync(userId, content);
        
        // Assert
        _messageMock.Verify(m => 
            m.PublishAsync(It.IsAny<TweetCreatedEvent>()), 
            Times.Once);
    }
    
    [Fact]
    public async Task CreateTweet_ContentTooLong_Throws()
    {
        // Arrange
        var content = new string('a', 281);
        
        // Act & Assert
        await Assert.ThrowsAsync<ArgumentException>(
            () => _tweetService.CreateTweetAsync(123, content));
    }
}
Checklist:

 Services tested in isolation
 External services mocked
 Business logic validated
 Error cases covered
Git Commit:

Bash

git add .
git commit -m "Week5-Day3-Evening: Microservice unit and integration tests"
git push origin main
THURSDAY, NOV 3
MORNING (8-10 AM): Saga Pattern
Video Search:

text

YouTube Search: "Saga pattern distributed transactions"

Duration: 40-45 minutes

Topics:
- Choreography vs Orchestration
- Compensating transactions
- Failure recovery
- Data consistency
Coding Task: Saga Implementation

Create file: Week5/SagaPattern.cs

csharp

// Saga for creating a user and initializing their data
public class UserRegistrationSaga
{
    private readonly IUserService _userService;
    private readonly IFeedService _feedService;
    private readonly INotificationService _notificationService;
    
    public async Task ExecuteAsync(string username, string email, string password)
    {
        try
        {
            // Step 1: Create user
            var user = await _userService.CreateUserAsync(username, email, password);
            
            // Step 2: Initialize feed
            await _feedService.InitializeFeedAsync(user.UserId);
            
            // Step 3: Send welcome notification
            await _notificationService.SendWelcomeEmailAsync(user.Email);
            
            // Success!
            Console.WriteLine("User registration completed");
        }
        catch (Exception ex)
        {
            // Compensate: Undo all changes
            await CompensateAsync(ex);
        }
    }
    
    private async Task CompensateAsync(Exception ex)
    {
        Console.WriteLine($"Registration failed: {ex.Message}. Compensating...");
        
        // Undo in reverse order
        // Step 3: Delete welcome notification (already sent, can't undo)
        
        // Step 2: Delete feed
        // await _feedService.DeleteFeedAsync(userId);
        
        // Step 1: Delete user
        // await _userService.DeleteUserAsync(userId);
        
        Console.WriteLine("Compensation complete");
    }
}

// Event-driven Saga (Choreography)
public class UserRegistrationEventHandler
{
    private readonly IServiceBus _serviceBus;
    
    [EventHandler]
    public async Task OnUserCreatedAsync(UserCreatedEvent @event)
    {
        // Publish event for next step
        await _serviceBus.PublishAsync(new UserRegistrationStep1Completed { UserId = @event.UserId });
    }
    
    [EventHandler]
    public async Task OnStep1CompletedAsync(UserRegistrationStep1Completed @event)
    {
        // Publish event for next step
        await _serviceBus.PublishAsync(new UserRegistrationStep2Completed { UserId = @event.UserId });
    }
}
Checklist:

 Saga flow defined
 Compensation logic implemented
 Error handling robust
 Event-driven saga option
Git Commit:

Bash

git add .
git commit -m "Week5-Day4-Morning: Saga pattern for distributed transactions"
git push origin main
EVENING (9-10 PM): Service Documentation
Coding Task: Service Catalog

Create file: Week5/SERVICE-CATALOG.md

Markdown

# TechTweet Service Catalog

## User Service

**Responsibility:** User identity and management

**Endpoints:**
- POST /api/users/register
- GET /api/users/{id}
- PUT /api/users/{id}/profile
- DELETE /api/users/{id}

**Database:** UserDB (SQL Server)

**Events Published:**
- UserRegistered
- UserProfileUpdated
- UserDeactivated

**Events Consumed:**
- None

**Dependencies:**
- None (leaf service)

---

## Tweet Service

**Responsibility:** Tweet creation and management

**Endpoints:**
- POST /api/tweets
- GET /api/tweets/{id}
- PUT /api/tweets/{id}
- DELETE /api/tweets/{id}

**Database:** TweetDB (SQL Server)

**Events Published:**
- TweetCreated
- TweetDeleted
- TweetUpdated

**Events Consumed:**
- UserDeactivated (cascade delete)

**Dependencies:**
- User Service (verify author)

---

## Feed Service

**Responsibility:** Feed generation and timeline

**Endpoints:**
- GET /api/feed/home
- POST /api/users/{id}/follow/{followId}
- DELETE /api/users/{id}/follow/{followId}

**Database:** FeedCache (Redis) + FeedDB (SQL Server)

**Events Published:**
- FeedGenerated
- UserFollowed

**Events Consumed:**
- TweetCreated
- UserFollowed

**Dependencies:**
- Tweet Service
- User Service

---

## Notification Service

**Responsibility:** Notifications delivery

**Endpoints:**
- GET /api/notifications
- PATCH /api/notifications/{id}/read

**Database:** NotificationDB (SQL Server)

**Events Published:**
- NotificationSent

**Events Consumed:**
- TweetCreated
- UserFollowed

**Dependencies:**
- User Service
- Email/SMS providers
Checklist:

 All services documented
 Dependencies clear
 Events mapped
 Endpoints listed
Git Commit:

Bash

git add .
git commit -m "Week5-Day4-Evening: Service catalog and documentation"
git push origin main
FRIDAY, NOV 5
MORNING (8-10 AM): Review
Checklist:

text

✓ Service boundaries defined
✓ Aggregates designed (DDD)
✓ Bounded contexts mapped
✓ Database per service
✓ Communication patterns
✓ Saga pattern
✓ Services documented
✓ Tests for isolation
EVENING (9-10 PM): Blog Post
Write: "Microservices Architecture with Domain-Driven Design"

Sections:

Service Decomposition (250 words)
Domain-Driven Design (250 words)
Database Strategy (200 words)
Communication Patterns (200 words)
Distributed Transactions (150 words)
Code Examples (300 words)
Publish on: Medium or Dev.to

Git Commit:

Bash

git add .
git commit -m "Week5 Complete: Microservices with DDD foundation"
git push origin main
Week 5 Summary:

text

✅ WEEK 5 COMPLETE!

What You Built:
✓ Service decomposition
✓ Domain-driven design
✓ Bounded contexts
✓ Database per service
✓ Service communication
✓ Saga pattern
✓ Service catalog

Metrics:
- Hours: 15/15 ✓
- Commits: 12+
- Blog posts: 5/5 ✓

Skills Improved:
- Architecture: 2.5/5 → 3.5/5
- Microservices: 0/5 → 3.5/5

Next Week: Docker & Kubernetes!
WEEK 6: DOCKER & KUBERNETES
Nov 6-12, 2026 | Goal: Containerize and orchestrate services
MONDAY, NOV 6
MORNING (8-10 AM): Docker Basics
Video Search:

text

YouTube Search: "TechWorld with Nana Docker tutorial for beginners"

Duration: 45-50 minutes
Creator: TechWorld with Nana

Topics:
- Containers vs VMs
- Images and layers
- Dockerfile basics
- Running containers
Coding Task: Dockerfile for .NET Service

Create file: Week6/Dockerfile

Dockerfile

# Multi-stage build

# Stage 1: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project files
COPY ["TechTweet.TweetService/TechTweet.TweetService.csproj", "TechTweet.TweetService/"]
RUN dotnet restore "TechTweet.TweetService/TechTweet.TweetService.csproj"

# Copy source code
COPY . .
WORKDIR "/src/TechTweet.TweetService"

# Build
RUN dotnet build "TechTweet.TweetService.csproj" -c Release -o /app/build

# Stage 2: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app

# Copy built app
COPY --from=build /app/build .

# Expose port
EXPOSE 5000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:5000/health/live || exit 1

# Run
ENTRYPOINT ["dotnet", "TechTweet.TweetService.dll"]
Checklist:

 Multi-stage build
 Optimized layers
 Health check included
 Ports exposed
Git Commit:

Bash

git add .
git commit -m "Week6-Day1-Morning: Dockerfile for .NET service"
git push origin main
EVENING (9-10 PM): Docker Compose
Video Search:

text

YouTube Search: "TechWorld with Nana Docker Compose tutorial"

Duration: 40-45 minutes
Creator: TechWorld with Nana

Topics:
- Multi-container setup
- Networking
- Volumes
- Environment variables
Coding Task: Docker Compose

Create file: Week6/docker-compose.yml

YAML

version: '3.8'

services:
  # SQL Server
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      ACCEPT_EULA: Y
      SA_PASSWORD: P@ssw0rd123!
    ports:
      - "1433:1433"
    volumes:
      - sqlserver_data:/var/opt/mssql/data
    networks:
      - techtwheel

  # Redis
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    networks:
      - techtwheel

  # RabbitMQ
  rabbitmq:
    image: rabbitmq:3.12-management-alpine
    environment:
      RABBITMQ_DEFAULT_USER: guest
      RABBITMQ_DEFAULT_PASS: guest
    ports:
      - "5672:5672"
      - "15672:15672"
    volumes:
      - rabbitmq_data:/var/lib/rabbitmq
    networks:
      - techtwheel

  # User Service
  user-service:
    build:
      context: .
      dockerfile: Dockerfile.UserService
    environment:
      - ConnectionStrings__DefaultConnection=Server=sqlserver;Database=Users;User Id=sa;Password=P@ssw0rd123!;
      - Redis__Host=redis
    ports:
      - "5001:5000"
    depends_on:
      - sqlserver
      - redis
    networks:
      - techtwheel

  # Tweet Service
  tweet-service:
    build:
      context: .
      dockerfile: Dockerfile.TweetService
    environment:
      - ConnectionStrings__DefaultConnection=Server=sqlserver;Database=Tweets;User Id=sa;Password=P@ssw0rd123!;
      - RabbitMQ__Host=rabbitmq
    ports:
      - "5002:5000"
    depends_on:
      - sqlserver
      - rabbitmq
    networks:
      - techtwheel

  # API Gateway
  api-gateway:
    build:
      context: .
      dockerfile: Dockerfile.Gateway
    environment:
      - Services__UserService=http://user-service:5000
      - Services__TweetService=http://tweet-service:5000
    ports:
      - "5000:5000"
    depends_on:
      - user-service
      - tweet-service
    networks:
      - techtwheel

volumes:
  sqlserver_data:
  redis_data:
  rabbitmq_data:

networks:
  techtwheel:
    driver: bridge
Run:

Bash

docker-compose up -d
# Verify all services running
docker-compose ps
# View logs
docker-compose logs -f
# Stop
docker-compose down
Checklist:

 docker-compose.yml created
 All services defined
 Networking configured
 Volumes for persistence
 Services can communicate
Git Commit:

Bash

git add .
git commit -m "Week6-Day1-Evening: Docker Compose for multi-container setup"
git push origin main
TUESDAY, NOV 7
MORNING (8-10 AM): Kubernetes Basics
Video Search:

text

YouTube Search: "TechWorld with Nana Kubernetes tutorial"

Duration: 50-60 minutes
Creator: TechWorld with Nana

Topics:
- Kubernetes architecture
- Pods, Deployments, Services
- kubectl commands
- Scaling and rolling updates
Coding Task: Kubernetes Manifests

Create file: Week6/k8s/deployment.yaml

YAML

apiVersion: apps/v1
kind: Deployment
metadata:
  name: tweet-service
  namespace: techtwheel
spec:
  replicas: 3  # Run 3 instances
  selector:
    matchLabels:
      app: tweet-service
  template:
    metadata:
      labels:
        app: tweet-service
    spec:
      containers:
      - name: tweet-service
        image: techtwheel/tweet-service:latest
        ports:
        - containerPort: 5000
        env:
        - name: ConnectionStrings__DefaultConnection
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: connection-string
        - name: RabbitMQ__Host
          value: rabbitmq-service
        
        # Resource requests and limits
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "500m"
        
        # Health checks
        livenessProbe:
          httpGet:
            path: /health/live
            port: 5000
          initialDelaySeconds: 30
          periodSeconds: 10
        
        readinessProbe:
          httpGet:
            path: /health/ready
            port: 5000
          initialDelaySeconds: 10
          periodSeconds: 5
        
        # Graceful shutdown
        lifecycle:
          preStop:
            exec:
              command: ["/bin/sh", "-c", "sleep 15"]
Create file: Week6/k8s/service.yaml

YAML

apiVersion: v1
kind: Service
metadata:
  name: tweet-service
  namespace: techtwheel
spec:
  type: ClusterIP
  selector:
    app: tweet-service
  ports:
  - port: 80
    targetPort: 5000
    name: http
Create file: Week6/k8s/configmap.yaml

YAML

apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: techtwheel
data:
  ASPNETCORE_ENVIRONMENT: "Production"
  LOG_LEVEL: "Information"
  CACHE_TTL: "3600"
Create file: Week6/k8s/secret.yaml

YAML

apiVersion: v1
kind: Secret
metadata:
  name: db-secret
  namespace: techtwheel
type: Opaque
stringData:
  connection-string: "Server=sqlserver;Database=Tweets;User Id=sa;Password=P@ssw0rd123!;"
  api-key: "your-secret-key-here"
Deploy to Kubernetes:

Bash

# Create namespace
kubectl create namespace techtwheel

# Apply manifests
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secret.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

# Verify deployment
kubectl get deployments -n techtwheel
kubectl get pods -n techtwheel
kubectl get services -n techtwheel

# View logs
kubectl logs -n techtwheel deployment/tweet-service

# Scale deployment
kubectl scale deployment tweet-service -n techtwheel --replicas=5

# Update image (rolling update)
kubectl set image deployment/tweet-service tweet-service=techtwheel/tweet-service:v2 -n techtwheel
Checklist:

 Deployment manifest created
 Service configured
 ConfigMap for settings
 Secrets for sensitive data
 Health checks defined
 Resources specified
 Can deploy to K8s
Git Commit:

Bash

git add .
git commit -m "Week6-Day2-Morning: Kubernetes manifests for service deployment"
git push origin main
EVENING (9-10 PM): ConfigMaps & Secrets
Video Search:

text

YouTube Search: "Kubernetes ConfigMaps and Secrets tutorial"

Duration: 30-35 minutes

Topics:
- Externalized configuration
- Secret management
- Volume mounting
- Environment variables
Coding Task: Configuration Management

Create file: Week6/k8s/hpa.yaml (Horizontal Pod Autoscaler):

YAML

apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: tweet-service-hpa
  namespace: techtwheel
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: tweet-service
  
  minReplicas: 3
  maxReplicas: 10
  
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
  
  behavior:
    scaleUp:
      stabilizationWindowSeconds: 0
      policies:
      - type: Percent
        value: 100
        periodSeconds: 15
    
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
      - type: Percent
        value: 10
        periodSeconds: 60
Checklist:

 ConfigMap working
 Secrets secure
 Environment variables set
 Auto-scaling configured
 Rolling updates work
Git Commit:

Bash

git add .
git commit -m "Week6-Day2-Evening: Auto-scaling and configuration management"
git push origin main
WEDNESDAY, NOV 8
MORNING (8-10 AM): Service Mesh (Optional)
Video Search:

text

YouTube Search: "Istio service mesh tutorial for beginners"

Duration: 40-45 minutes

Topics:
- Sidecar proxies
- Traffic management
- Security policies
- Observability
Coding Task: Istio VirtualService

Create file: Week6/k8s/virtualservice.yaml

YAML

apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: tweet-service
  namespace: techtwheel
spec:
  hosts:
  - tweet-service
  http:
  - match:
    - uri:
        prefix: "/api/tweets"
    route:
    - destination:
        host: tweet-service
        port:
          number: 80
      weight: 90
    - destination:
        host: tweet-service
        port:
          number: 80
      weight: 10
    timeout: 30s
    retries:
      attempts: 3
      perTryTimeout: 10s

---
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: tweet-service
  namespace: techtwheel
spec:
  host: tweet-service
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 100
      http:
        http1MaxPendingRequests: 100
    outlierDetection:
      consecutive5xxErrors: 5
      interval: 30s
      baseEjectionTime: 30s
Checklist:

 VirtualService configured
 Traffic routing working
 Retries and timeouts set
 Circuit breaking configured
Git Commit:

Bash

git add .
git commit -m "Week6-Day3-Morning: Istio service mesh configuration"
git push origin main
EVENING (9-10 PM): Monitoring & Observability
Video Search:

text

YouTube Search: "Prometheus and Grafana monitoring Kubernetes"

Duration: 35-40 minutes

Topics:
- Metrics collection
- Dashboards
- Alerting
- Application insights
Coding Task: Prometheus ServiceMonitor

Create file: Week6/k8s/servicemonitor.yaml

YAML

apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: tweet-service-monitor
  namespace: techtwheel
spec:
  selector:
    matchLabels:
      app: tweet-service
  endpoints:
  - port: http
    interval: 30s
    path: /metrics
Create file: Week6/k8s/alert-rules.yaml

YAML

apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: tweet-service-alerts
  namespace: techtwheel
spec:
  groups:
  - name: tweet-service
    interval: 30s
    rules:
    - alert: HighErrorRate
      expr: rate(http_request_errors_total[5m]) > 0.05
      for: 5m
      annotations:
        summary: "High error rate in tweet-service"
    
    - alert: HighLatency
      expr: histogram_quantile(0.95, http_request_duration_seconds) > 1
      for: 5m
      annotations:
        summary: "High latency in tweet-service"
    
    - alert: PodCrashLooping
      expr: rate(kube_pod_container_status_restarts_total[15m]) > 0.1
      for: 5m
      annotations:
        summary: "Pod is crash looping"
Checklist:

 Prometheus metrics exposed
 ServiceMonitor configured
 Grafana dashboards created
 Alert rules defined
 Notifications tested
Git Commit:

Bash

git add .
git commit -m "Week6-Day3-Evening: Prometheus monitoring and alerting"
git push origin main
THURSDAY, NOV 9
MORNING (8-10 AM): Ingress & Networking
Video Search:

text

YouTube Search: "Kubernetes Ingress tutorial"

Duration: 35-40 minutes

Topics:
- Ingress controllers
- Path-based routing
- TLS/SSL
- Load balancing
Coding Task: Ingress Configuration

Create file: Week6/k8s/ingress.yaml

YAML

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: techtwheel-ingress
  namespace: techtwheel
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - api.techtwheel.com
    secretName: techtwheel-tls
  
  rules:
  - host: api.techtwheel.com
    http:
      paths:
      - path: /api/auth
        pathType: Prefix
        backend:
          service:
            name: auth-service
            port:
              number: 80
      
      - path: /api/tweets
        pathType: Prefix
        backend:
          service:
            name: tweet-service
            port:
              number: 80
      
      - path: /api/feed
        pathType: Prefix
        backend:
          service:
            name: feed-service
            port:
              number: 80
Checklist:

 Ingress controller installed
 Routes configured
 TLS certificates issued
 Domain resolving
Git Commit:

Bash

git add .
git commit -m "Week6-Day4-Morning: Kubernetes Ingress with TLS"
git push origin main
EVENING (9-10 PM): Documentation
Coding Task: K8s Operations Guide

Create file: Week6/K8S-OPERATIONS-GUIDE.md

Markdown

# Kubernetes Operations Guide

## Prerequisites
- kubectl installed and configured
- Access to K8s cluster
- Docker images pushed to registry

## Deployment

### Step 1: Create Namespace
```bash
kubectl create namespace techtwheel
Step 2: Apply ConfigMaps & Secrets
Bash

kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secret.yaml
Step 3: Deploy Services
Bash

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
Step 4: Setup Ingress
Bash

kubectl apply -f k8s/ingress.yaml
Step 5: Configure Autoscaling
Bash

kubectl apply -f k8s/hpa.yaml
Monitoring
View Logs
Bash

kubectl logs -n techtwheel deployment/tweet-service
kubectl logs -n techtwheel -f pod/tweet-service-xxxxx  # Follow logs
Check Pod Status
Bash

kubectl get pods -n techtwheel
kubectl describe pod tweet-service-xxxxx -n techtwheel
View Metrics
Bash

kubectl top pods -n techtwheel
kubectl top nodes
Troubleshooting
Pod not starting
Bash

kubectl describe pod POD_NAME -n techtwheel
kubectl logs POD_NAME -n techtwheel
Service not accessible
Bash

kubectl get service -n techtwheel
kubectl port-forward service/tweet-service 8000:80 -n techtwheel
Resource limits reached
Bash

kubectl edit deployment tweet-service -n techtwheel
# Increase memory/CPU limits
Scaling
Manual Scale
Bash

kubectl scale deployment tweet-service --replicas=5 -n techtwheel
Auto-scaling Status
Bash

kubectl get hpa -n techtwheel
kubectl describe hpa tweet-service-hpa -n techtwheel
Updates
Rolling Update
Bash

kubectl set image deployment/tweet-service tweet-service=techtwheel/tweet-service:v2 -n techtwheel
kubectl rollout status deployment/tweet-service -n techtwheel
Rollback
Bash

kubectl rollout history deployment/tweet-service -n techtwheel
kubectl rollout undo deployment/tweet-service -n techtwheel
text


**Checklist:**
- [ ] Operations guide complete
- [ ] Common commands documented
- [ ] Troubleshooting tips included
- [ ] Scaling procedures clear

**Git Commit:**
```bash
git add .
git commit -m "Week6-Day4-Evening: Kubernetes operations documentation"
git push origin main
FRIDAY, NOV 10
MORNING (8-10 AM): Review
Checklist:

text

✓ Dockerfile created
✓ Docker Compose working
✓ Kubernetes manifests
✓ Services deployable
✓ ConfigMaps & Secrets
✓ Auto-scaling configured
✓ Ingress setup
✓ Monitoring ready
EVENING (9-10 PM): Blog Post
Write: "Containerizing and Orchestrating Microservices with K8s"

Sections:

Docker Fundamentals (250 words)
Multi-stage Builds (200 words)
Kubernetes Architecture (250 words)
Deployment Strategies (200 words)
Auto-scaling & Monitoring (150 words)
Code Examples (300 words)
Publish on: Medium or Dev.to

Git Commit:

Bash

git add .
git commit -m "Week6 Complete: Docker and Kubernetes containerization"
git push origin main
Week 6 Summary:

text

✅ WEEK 6 COMPLETE!

What You Built:
✓ Production Dockerfile
✓ Docker Compose setup
✓ Kubernetes Deployments
✓ Service manifests
✓ ConfigMaps & Secrets
✓ Horizontal Pod Autoscaler
✓ Ingress configuration
✓ Monitoring setup

Metrics:
- Hours: 15/15 ✓
- Commits: 14+
- Blog posts: 6/6 ✓

Skills Improved:
- Docker: 0/5 → 3.5/5
- Kubernetes: 0/5 → 3.5/5
- DevOps: 0/5 → 3/5

Next Week: Azure Cloud Deployment!

 Technical discussions ready
Final Blog Post:
"My 12-Week Journey: Developer → Architect"

WEEK 7: AZURE DEPLOYMENT
Nov 9-15, 2026 | Goal: Deploy microservices to Azure cloud
MONDAY, NOV 9
MORNING (8-10 AM): Azure Fundamentals
Video Search:

text

YouTube Search 1: "John Savill Azure fundamentals for beginners"
YouTube Search 2: "TechWorld with Nana Azure tutorial"

Duration: 45-50 minutes
Creator: John Savill or TechWorld with Nana

Topics:
- Azure regions and availability zones
- Resource Groups organization
- Subscription management
- Azure Portal navigation
- Cost estimation
Coding Task: Create Azure Resources

Create file: Week7/Azure-Setup-Guide.md

Markdown

# Azure Deployment Setup

## Prerequisites
1. Azure Account: https://azure.microsoft.com/free
2. Azure CLI: https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
3. Azure PowerShell (optional)

## Step 1: Create Resource Group

```bash
# Login to Azure
az login

# Create resource group
az group create \
  --name techtwheel-rg \
  --location eastus

# Verify creation
az group list --output table
Step 2: Create Storage Account (for backups)
Bash

az storage account create \
  --resource-group techtwheel-rg \
  --name techtweetstorage \
  --location eastus \
  --sku Standard_LRS

# Get connection string
az storage account show-connection-string \
  --resource-group techtwheel-rg \
  --name techtweetstorage
Step 3: Create Key Vault (for secrets)
Bash

az keyvault create \
  --resource-group techtwheel-rg \
  --name techtwheel-vault \
  --location eastus

# Add secrets
az keyvault secret set \
  --vault-name techtwheel-vault \
  --name DatabasePassword \
  --value "YourSecurePassword123!"

az keyvault secret set \
  --vault-name techtwheel-vault \
  --name ApiKey \
  --value "your-api-key-here"

# List secrets
az keyvault secret list \
  --vault-name techtwheel-vault
Step 4: Create App Service Plan
Bash

# Standard tier for production
az appservice plan create \
  --name techtwheel-plan \
  --resource-group techtwheel-rg \
  --sku S1 \
  --is-linux

# View available SKUs
az appservice plan list-skus \
  --resource-group techtwheel-rg
Costs Estimation
Resource	SKU	Monthly Cost
Resource Group	Free	$0
Storage Account	Standard LRS	$1-5
Key Vault	Standard	$0.6
App Service Plan S1	Standard	$65-75
Azure SQL (2vCore)	Standard	$350-400
Service Bus	Standard	$10-20
Total Estimated		$425-500
Cost Optimization Tips
Use F1 (Free) tier for development
Stop resources when not in use
Use Reserved Instances for 1-year commitment
Monitor spending with Azure Cost Management
Use Spot VMs for non-critical workloads
text


**Checklist:**
- [ ] Azure account created
- [ ] Resource group created
- [ ] Key Vault setup
- [ ] Storage account created
- [ ] App Service plan created
- [ ] Understand Azure pricing

**Git Commit:**
```bash
git add .
git commit -m "Week7-Day1-Morning: Azure account setup and resource group"
git push origin main
EVENING (9-10 PM): App Service Deployment
Video Search:

text

YouTube Search: "Deploy .NET Core to Azure App Service"

Duration: 35-40 minutes
Creator: John Savill or Code Maze

Topics:
- Publishing .NET apps to App Service
- Deployment slots (staging/production)
- Auto-scaling configuration
- Environment variables
- Connection strings
Coding Task: Publish to App Service

Bash

# Step 1: Create App Service Web App
az webapp create \
  --resource-group techtwheel-rg \
  --plan techtwheel-plan \
  --name techtwheel-api \
  --runtime "DOTNETCORE:8.0"

# Step 2: Configure connection strings
az webapp config connection-string set \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --settings \
    DefaultConnection="Server=tcp:techtwheel-server.database.windows.net;" \
    "Database=TechTweet;User Id=sqladmin;Password=YourPassword123!@" \
  --connection-string-type SQLServer

# Step 3: Configure app settings (environment variables)
az webapp config appsettings set \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --settings \
    ASPNETCORE_ENVIRONMENT=Production \
    Jwt__Secret=your-long-secret-key \
    Redis__Host=techtwheel-redis.redis.cache.azure.com \
    ServiceBus__ConnectionString=Endpoint=sb://...

# Step 4: Publish from Visual Studio
# In Visual Studio:
# Right-click project → Publish → Azure → App Service
# Select techtwheel-api
# Click Publish

# OR via CLI:
cd /path/to/project
dotnet publish -c Release -o ./publish

# Install Azure App Service extension
dotnet tool install -g azure-functions-core-tools

# Deploy using Azure CLI
az webapp deployment source config-zip \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --src ./publish.zip

# Step 5: Verify deployment
az webapp show \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --query "{defaultHostName:defaultHostName,state:state}"

# Visit: https://techtwheel-api.azurewebsites.net
Create Deployment Slot (Staging)

Bash

# Create staging slot
az webapp deployment slot create \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --slot staging

# Deploy to staging first
az webapp deployment source config-zip \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --slot staging \
  --src ./publish.zip

# Test staging: https://techtwheel-api-staging.azurewebsites.net

# Swap to production (zero downtime)
az webapp deployment slot swap \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --slot staging
Configure Auto-Scaling

Bash

# Create auto-scale rule
az monitor autoscale create \
  --resource-group techtwheel-rg \
  --resource-name techtwheel-plan \
  --resource-type "Microsoft.Web/serverfarms" \
  --min-count 2 \
  --max-count 5 \
  --count 2

# Add scale-out rule (CPU > 70%)
az monitor autoscale rule create \
  --resource-group techtwheel-rg \
  --autoscale-name techtwheel-autoscale \
  --condition "Percentage CPU > 70 avg 5m" \
  --scale out 1

# Add scale-in rule (CPU < 30%)
az monitor autoscale rule create \
  --resource-group techtwheel-rg \
  --autoscale-name techtwheel-autoscale \
  --condition "Percentage CPU < 30 avg 5m" \
  --scale in 1
Checklist:

 App Service created
 App deployed successfully
 Can access via HTTPS
 Staging slot configured
 Auto-scaling enabled
 Environment variables set
Git Commit:

Bash

git add .
git commit -m "Week7-Day1-Evening: Deploy to Azure App Service with slots"
git push origin main
TUESDAY, NOV 10
MORNING (8-10 AM): Azure SQL Database
Video Search:

text

YouTube Search 1: "John Savill Azure SQL Database tutorial"
YouTube Search 2: "Deploy SQL Server to Azure"

Duration: 45-50 minutes
Creator: John Savill

Topics:
- Azure SQL Database options (Single, Elastic Pool)
- DTU vs vCore pricing models
- Backup and restore
- Geo-replication
- Managed backups
Coding Task: Create and Configure Azure SQL

Bash

# Step 1: Create Azure SQL Server
az sql server create \
  --name techtwheel-server \
  --resource-group techtwheel-rg \
  --location eastus \
  --admin-user sqladmin \
  --admin-password "YourPassword123!@"

# Step 2: Create Azure SQL Database
az sql db create \
  --resource-group techtwheel-rg \
  --server techtwheel-server \
  --name TechTweet \
  --service-objective S2 \
  --backup-storage-redundancy Geo

# Step 3: Configure firewall (allow Azure services)
az sql server firewall-rule create \
  --resource-group techtwheel-rg \
  --server techtwheel-server \
  --name "AllowAzureServices" \
  --start-ip-address 0.0.0.0 \
  --end-ip-address 0.0.0.0

# Step 4: Allow your IP (for local development)
az sql server firewall-rule create \
  --resource-group techtwheel-rg \
  --server techtwheel-server \
  --name "AllowMyIP" \
  --start-ip-address YOUR_PUBLIC_IP \
  --end-ip-address YOUR_PUBLIC_IP

# Step 5: Get connection string
az sql db show-connection-string \
  --server techtwheel-server \
  --name TechTweet \
  --client ado.net

# Step 6: Configure long-term retention (backup)
az sql db ltr-backup-set \
  --resource-group techtwheel-rg \
  --server techtwheel-server \
  --database TechTweet \
  --weekly-retention P52W  # 52 weeks
Update appsettings.json:

JSON

{
  "ConnectionStrings": {
    "DefaultConnection": "Server=tcp:techtwheel-server.database.windows.net,1433;Initial Catalog=TechTweet;Persist Security Info=False;User ID=sqladmin;Password=YourPassword123!@;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
  }
}
Migrate Database:

Bash

# Install EF Core tools
dotnet tool install --global dotnet-ef

# Update database schema
dotnet ef database update --connection "Server=tcp:techtwheel-server.database.windows.net,1433;Initial Catalog=TechTweet;User ID=sqladmin;Password=YourPassword123!@;Encrypt=true;"

# OR if using migrations
dotnet ef migrations script --output migration.sql
# Then run script in SQL Management Studio
Enable Geo-Replication (High Availability)

Bash

# Create secondary database (different region)
az sql db replica create \
  --resource-group techtwheel-rg \
  --server techtwheel-server \
  --name TechTweet \
  --partner-server techtwheel-server-geo \
  --partner-resource-group techtwheel-rg-geo

# Failover if primary fails
az sql db replica failover \
  --resource-group techtwheel-rg-geo \
  --server techtwheel-server-geo \
  --name TechTweet
Checklist:

 Azure SQL Server created
 Database created (TechTweet)
 Firewall rules configured
 Connection string obtained
 Local development can connect
 App Service can connect
 Backups enabled
 Geo-replication setup
Git Commit:

Bash

git add .
git commit -m "Week7-Day2-Morning: Azure SQL Database setup with backups"
git push origin main
EVENING (9-10 PM): Managed Identity & Security
Video Search:

text

YouTube Search: "Azure Managed Identity authentication"

Duration: 35-40 minutes

Topics:
- System-assigned vs User-assigned identities
- Accessing Azure SQL without passwords
- Accessing Key Vault securely
- RBAC (Role-Based Access Control)
Coding Task: Configure Managed Identity

Bash

# Step 1: Enable Managed Identity on App Service
az webapp identity assign \
  --resource-group techtwheel-rg \
  --name techtwheel-api

# Get the identity principal ID
az webapp identity show \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --query principalId

# Step 2: Grant SQL Database access to identity
# Get principal ID from above
PRINCIPAL_ID="your-principal-id"

# Create SQL database user for the identity
# Connect to Azure SQL and run:
/*
CREATE USER [techtwheel-api] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [techtwheel-api];
ALTER ROLE db_datawriter ADD MEMBER [techtwheel-api];
ALTER ROLE db_ddladmin ADD MEMBER [techtwheel-api];
*/

# Step 3: Grant Key Vault access to identity
az keyvault set-policy \
  --name techtwheel-vault \
  --object-id $PRINCIPAL_ID \
  --secret-permissions get list \
  --key-permissions get list

# Step 4: Update connection string (remove password)
az webapp config connection-string set \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --settings \
    DefaultConnection="Server=tcp:techtwheel-server.database.windows.net,1433;Initial Catalog=TechTweet;Encrypt=True;TrustServerCertificate=False;" \
  --connection-string-type SQLServer

# Step 5: Update code to use Managed Identity
# In Program.cs:
/*
var tokenProvider = new DefaultAzureCredential();
var token = await tokenProvider.GetTokenAsync(
    new TokenRequestContext(new[] { "https://database.windows.net/.default" }));

var connection = new SqlConnection(connectionString)
{
    AccessToken = token.Token
};
*/
Update Program.cs for Managed Identity:

csharp

using Azure.Identity;
using Azure.Security.KeyVault.Secrets;
using Microsoft.Data.SqlClient;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// Use Managed Identity for Azure SQL
var keyVaultUrl = new Uri("https://techtwheel-vault.vault.azure.net/");
var secretClient = new SecretClient(keyVaultUrl, new DefaultAzureCredential());

// Get secrets from Key Vault
var jwtSecret = secretClient.GetSecret("JwtSecret");
var databaseUrl = secretClient.GetSecret("DatabaseUrl");

// Configure SQL Connection with Managed Identity
var sqlConnectionStringBuilder = new SqlConnectionStringBuilder(
    builder.Configuration.GetConnectionString("DefaultConnection"))
{
    Authentication = SqlAuthenticationMethod.ActiveDirectoryDefault,
    Encrypt = true,
    TrustServerCertificate = false,
    ConnectRetryCount = 3,
    ConnectRetryInterval = 10
};

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(sqlConnectionStringBuilder.ConnectionString));

// Use Key Vault for secrets
builder.Services.AddSingleton(secretClient);

var app = builder.Build();

// ... rest of configuration
Checklist:

 Managed Identity enabled on App Service
 SQL database user created for identity
 Key Vault access granted
 Connection string updated (no password)
 Code updated to use Managed Identity
 No secrets in appsettings.json
 Secrets retrieved from Key Vault
Git Commit:

Bash

git add .
git commit -m "Week7-Day2-Evening: Managed Identity for passwordless access"
git push origin main
WEDNESDAY, NOV 11
MORNING (8-10 AM): Azure Service Bus
Video Search:

text

YouTube Search: "Azure Service Bus queues and topics"

Duration: 40-45 minutes

Topics:
- Queues vs Topics
- Dead-letter queues
- Message sessions
- Partitioning
Coding Task: Migrate to Azure Service Bus

Bash

# Step 1: Create Service Bus Namespace
az servicebus namespace create \
  --resource-group techtwheel-rg \
  --name techtwheel-bus \
  --location eastus \
  --sku Standard

# Step 2: Create Queue
az servicebus queue create \
  --resource-group techtwheel-rg \
  --namespace-name techtwheel-bus \
  --name tweet-creation \
  --default-message-ttl PT1H \
  --max-delivery-count 3

# Step 3: Create Topic (for pub/sub)
az servicebus topic create \
  --resource-group techtwheel-rg \
  --namespace-name techtwheel-bus \
  --name tweet-events

# Step 4: Create Subscriptions to Topic
az servicebus topic subscription create \
  --resource-group techtwheel-rg \
  --namespace-name techtwheel-bus \
  --topic-name tweet-events \
  --name feed-service

az servicebus topic subscription create \
  --resource-group techtwheel-rg \
  --namespace-name techtwheel-bus \
  --topic-name tweet-events \
  --name notification-service

# Step 5: Get connection string
az servicebus namespace authorization-rule keys list \
  --resource-group techtwheel-rg \
  --namespace-name techtwheel-bus \
  --name RootManageSharedAccessKey \
  --query primaryConnectionString

# Step 6: Update app settings
az webapp config appsettings set \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --settings \
    ServiceBus__ConnectionString="your-connection-string"
Update Code to Use Azure Service Bus:

Create file: Week7/AzureServiceBusConfig.cs

csharp

using Azure.Messaging.ServiceBus;
using Microsoft.Extensions.Azure;
using Microsoft.Extensions.DependencyInjection;

public static class ServiceBusExtensions
{
    public static IServiceCollection AddAzureServiceBus(
        this IServiceCollection services,
        string connectionString)
    {
        // Add Service Bus client
        services.AddAzureClients(builder =>
        {
            builder.AddServiceBusClient(connectionString);
        });
        
        // Register publishers and handlers
        services.AddScoped<ITweetEventPublisher, AzureServiceBusTweetPublisher>();
        services.AddScoped<ITweetEventHandler, AzureServiceBusTweetHandler>();
        
        // Register hosted services for consuming messages
        services.AddHostedService<FeedServiceMessageHandler>();
        services.AddHostedService<NotificationServiceMessageHandler>();
        
        return services;
    }
}

// Publisher
public class AzureServiceBusTweetPublisher : ITweetEventPublisher
{
    private readonly ServiceBusClient _client;
    
    public AzureServiceBusTweetPublisher(ServiceBusClient client)
    {
        _client = client;
    }
    
    public async Task PublishTweetCreatedAsync(Tweet tweet)
    {
        var sender = _client.CreateSender("tweet-events");
        
        var message = new ServiceBusMessage(
            JsonSerializer.Serialize(new TweetCreatedEvent
            {
                TweetId = tweet.TweetId,
                UserId = tweet.UserId,
                Content = tweet.Content,
                CreatedAt = tweet.CreatedAt
            }))
        {
            Subject = "TweetCreated",
            SessionId = tweet.UserId.ToString()
        };
        
        await sender.SendMessageAsync(message);
        await sender.DisposeAsync();
    }
}

// Consumer (Hosted Service)
public class FeedServiceMessageHandler : BackgroundService
{
    private readonly ServiceBusClient _client;
    private readonly ILogger<FeedServiceMessageHandler> _logger;
    
    public FeedServiceMessageHandler(
        ServiceBusClient client,
        ILogger<FeedServiceMessageHandler> logger)
    {
        _client = client;
        _logger = logger;
    }
    
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        var processor = _client.CreateProcessor(
            "tweet-events",
            "feed-service",
            new ServiceBusProcessorOptions());
        
        processor.ProcessMessageAsync += async args =>
        {
            try
            {
                var eventBody = args.Message.Body.ToString();
                var tweetEvent = JsonSerializer.Deserialize<TweetCreatedEvent>(eventBody);
                
                _logger.LogInformation("Feed service processing tweet from user {UserId}", 
                    tweetEvent.UserId);
                
                // Update feed...
                
                await args.CompleteMessageAsync(args.Message);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Error processing message");
                await args.DeadLetterMessageAsync(args.Message);
            }
        };
        
        processor.ProcessErrorAsync += async args =>
        {
            _logger.LogError(args.Exception, "Error in message processor");
            await Task.CompletedTask;
        };
        
        await processor.StartProcessingAsync(stoppingToken);
        
        while (!stoppingToken.IsCancellationRequested)
        {
            await Task.Delay(1000, stoppingToken);
        }
        
        await processor.StopProcessingAsync();
        await processor.DisposeAsync();
    }
}
Update Program.cs:

csharp

var serviceBusConnectionString = builder.Configuration.GetConnectionString("ServiceBus");
builder.Services.AddAzureServiceBus(serviceBusConnectionString);

// ... rest of configuration
Checklist:

 Service Bus namespace created
 Queue created for commands
 Topic created for events
 Subscriptions created
 Connection string added to Key Vault
 Code updated to use Azure Service Bus
 Publishers and consumers working
 Message routing tested
Git Commit:

Bash

git add .
git commit -m "Week7-Day3-Morning: Azure Service Bus integration"
git push origin main
EVENING (9-10 PM): Azure Cache for Redis
Video Search:

text

YouTube Search: "Azure Cache for Redis tutorial"

Duration: 30-35 minutes

Topics:
- Redis data structures
- Expiration policies
- Clustering
- Performance monitoring
Coding Task: Setup Azure Redis Cache

Bash

# Step 1: Create Redis Cache
az redis create \
  --resource-group techtwheel-rg \
  --name techtwheel-redis \
  --location eastus \
  --sku Basic \
  --vm-size c0

# Step 2: Enable SSL/TLS
az redis update \
  --resource-group techtwheel-rg \
  --name techtwheel-redis \
  --minimum-tls-version 1.2

# Step 3: Get connection string
az redis show-connection-string \
  --name techtwheel-redis \
  --resource-group techtwheel-rg

# Step 4: Add to Key Vault
az keyvault secret set \
  --vault-name techtwheel-vault \
  --name RedisConnectionString \
  --value "your-redis-connection-string"

# Step 5: Update app settings
az webapp config appsettings set \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --settings \
    Redis__ConnectionString="@Microsoft.KeyVault(SecretUri=https://techtwheel-vault.vault.azure.net/secrets/RedisConnectionString/)"
Update Code:

csharp

using StackExchange.Redis;

// In Program.cs
var redisConnectionString = configuration.GetConnectionString("Redis");
var redis = ConnectionMultiplexer.Connect(redisConnectionString);

services.AddSingleton<IConnectionMultiplexer>(redis);
services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = redisConnectionString;
});

// Use in service
public class CacheService
{
    private readonly IDistributedCache _cache;
    
    public CacheService(IDistributedCache cache)
    {
        _cache = cache;
    }
    
    public async Task SetAsync<T>(string key, T value, TimeSpan? expiration = null)
    {
        var json = JsonSerializer.Serialize(value);
        await _cache.SetStringAsync(key, json, new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = expiration ?? TimeSpan.FromHours(1)
        });
    }
    
    public async Task<T> GetAsync<T>(string key)
    {
        var json = await _cache.GetStringAsync(key);
        return json == null ? default : JsonSerializer.Deserialize<T>(json);
    }
}
Checklist:

 Redis Cache created in Azure
 TLS/SSL enabled
 Connection string added to Key Vault
 App can connect to Redis
 Caching working
 Performance improved
Git Commit:

Bash

git add .
git commit -m "Week7-Day3-Evening: Azure Cache for Redis implementation"
git push origin main
THURSDAY, NOV 12
MORNING (8-10 AM): Azure Key Vault
Video Search:

text

YouTube Search: "Azure Key Vault secrets management tutorial"

Duration: 40-45 minutes

Topics:
- Secret management
- Key rotation
- Access policies
- Monitoring and auditing
Coding Task: Centralized Secrets Management

Create file: Week7/KeyVaultConfiguration.cs

csharp

using Azure.Identity;
using Azure.Security.KeyVault.Secrets;
using Microsoft.Extensions.Configuration;

public static class KeyVaultExtensions
{
    public static ConfigurationBuilder AddAzureKeyVault(
        this ConfigurationBuilder builder,
        string vaultName)
    {
        var kvUri = $"https://{vaultName}.vault.azure.net";
        var credential = new DefaultAzureCredential();
        
        builder.AddAzureKeyVault(
            new Uri(kvUri),
            credential,
            new KeyVaultSecretManager());
        
        return builder;
    }
}

// In Program.cs
var builder = WebApplication.CreateBuilder(args);

// Add Key Vault to configuration
var keyVaultName = builder.Configuration["KeyVaultName"] ?? "techtwheel-vault";
builder.Configuration.AddAzureKeyVault(keyVaultName);

// Now you can access secrets like:
var jwtSecret = builder.Configuration["JwtSecret"];
var dbConnectionString = builder.Configuration["DatabaseConnectionString"];
var redisConnectionString = builder.Configuration["RedisConnectionString"];
var serviceBusConnectionString = builder.Configuration["ServiceBusConnectionString"];
Store All Secrets:

Bash

# Database
az keyvault secret set \
  --vault-name techtwheel-vault \
  --name DatabaseConnectionString \
  --value "Server=tcp:techtwheel-server.database.windows.net,1433;Initial Catalog=TechTweet;..."

# JWT
az keyvault secret set \
  --vault-name techtwheel-vault \
  --name JwtSecret \
  --value "your-very-long-secret-key-min-32-characters"

# API Keys
az keyvault secret set \
  --vault-name techtwheel-vault \
  --name EmailApiKey \
  --value "sendgrid-api-key"

# OAuth Credentials
az keyvault secret set \
  --vault-name techtwheel-vault \
  --name GoogleClientId \
  --value "your-google-client-id"

az keyvault secret set \
  --vault-name techtwheel-vault \
  --name GoogleClientSecret \
  --value "your-google-client-secret"

# List all secrets
az keyvault secret list \
  --vault-name techtwheel-vault \
  --output table
Enable Key Rotation:

Bash

# Create rotation policy (1 year)
az keyvault secret set \
  --vault-name techtwheel-vault \
  --name JwtSecret \
  --value "new-rotated-secret" \
  --expires 31536000  # 1 year in seconds

# Enable audit logging
az monitor diagnostic-settings create \
  --resource-group techtwheel-rg \
  --resource techtwheel-vault \
  --resource-type "Microsoft.KeyVault/vaults" \
  --name "KeyVaultAudit" \
  --workspace "/subscriptions/your-subscription-id/resourcegroups/techtwheel-rg/providers/microsoft.operationalinsights/workspaces/techtwheel-logs" \
  --logs '[{"category":"AuditEvent","enabled":true}]'
Checklist:

 All secrets in Key Vault (no hardcoded secrets)
 App can read from Key Vault
 Managed Identity has access
 Audit logging enabled
 Rotation policy set
 Secrets never in config files or logs
Git Commit:

Bash

git add .
git commit -m "Week7-Day4-Morning: Azure Key Vault centralized secrets"
git push origin main
EVENING (9-10 PM): Monitoring and Diagnostics
Video Search:

text

YouTube Search: "Azure Application Insights monitoring"

Duration: 40-45 minutes

Topics:
- Application Insights setup
- Custom metrics and events
- Log Analytics
- Alerting rules
Coding Task: Setup Application Insights

Bash

# Step 1: Create Application Insights
az monitor app-insights component create \
  --resource-group techtwheel-rg \
  --app techtwheel-insights \
  --location eastus \
  --kind web \
  --application-type web

# Step 2: Get instrumentation key
az monitor app-insights component show \
  --resource-group techtwheel-rg \
  --app techtwheel-insights \
  --query instrumentationKey

# Step 3: Store in Key Vault
az keyvault secret set \
  --vault-name techtwheel-vault \
  --name AppInsightsKey \
  --value "your-instrumentation-key"

# Step 4: Add to app settings
az webapp config appsettings set \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --settings \
    APPINSIGHTS_INSTRUMENTATIONKEY="@Microsoft.KeyVault(SecretUri=https://techtwheel-vault.vault.azure.net/secrets/AppInsightsKey/)" \
    ApplicationInsightsAgent_EXTENSION_VERSION="~3"
Configure in Code:

csharp

// Install NuGet: Microsoft.ApplicationInsights.AspNetCore

builder.Services.AddApplicationInsightsTelemetry();

var app = builder.Build();

// Application Insights is now automatically tracking:
// - HTTP requests
// - Dependencies (DB, external APIs)
// - Exceptions
// - Performance counters

// Add custom metrics
public class OrderService
{
    private readonly TelemetryClient _telemetryClient;
    
    public OrderService(TelemetryClient telemetryClient)
    {
        _telemetryClient = telemetryClient;
    }
    
    public async Task<Order> CreateOrderAsync(Order order)
    {
        using (var operation = _telemetryClient.StartOperation<RequestTelemetry>("CreateOrder"))
        {
            try
            {
                var stopwatch = Stopwatch.StartNew();
                
                // Business logic
                var createdOrder = await _db.Orders.AddAsync(order);
                await _db.SaveChangesAsync();
                
                stopwatch.Stop();
                
                // Track custom metric
                _telemetryClient.TrackEvent("OrderCreated", 
                    new Dictionary<string, string>
                    {
                        { "OrderId", order.OrderId.ToString() },
                        { "UserId", order.UserId.ToString() }
                    },
                    new Dictionary<string, double>
                    {
                        { "ProcessingTime", stopwatch.ElapsedMilliseconds }
                    });
                
                return createdOrder;
            }
            catch (Exception ex)
            {
                _telemetryClient.TrackException(ex);
                throw;
            }
        }
    }
}
Create Alert Rules:

Bash

# Alert: High error rate
az monitor metrics alert create \
  --resource-group techtwheel-rg \
  --scopes /subscriptions/your-subscription-id/resourceGroups/techtwheel-rg/providers/microsoft.insights/components/techtwheel-insights \
  --name "HighErrorRate" \
  --description "Alert when error rate > 5%" \
  --condition "avg failed_requests_percentage > 5" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --severity 3

# Alert: High latency
az monitor metrics alert create \
  --resource-group techtwheel-rg \
  --scopes /subscriptions/your-subscription-id/resourceGroups/techtwheel-rg/providers/microsoft.insights/components/techtwheel-insights \
  --name "HighLatency" \
  --description "Alert when response time > 1 second" \
  --condition "avg server_response_time > 1000" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --severity 2
View Metrics in Azure Portal:

text

1. Go to: https://portal.azure.com
2. Search: "Application Insights"
3. Select: "techtwheel-insights"
4. View:
   - Performance/Failures/Users
   - Real-time metrics
   - Live metric stream
   - Alerts
Checklist:

 Application Insights created
 Instrumentation key configured
 App automatically tracking metrics
 Custom events/metrics working
 Alert rules created
 Can view dashboards in portal
Git Commit:

Bash

git add .
git commit -m "Week7-Day4-Evening: Application Insights monitoring setup"
git push origin main
FRIDAY, NOV 13
MORNING (8-10 AM): Production Security Review
Coding Task: Security Checklist

Create file: Week7/PRODUCTION-SECURITY-CHECKLIST.md

Markdown

# Production Security Checklist

## Network Security
- [ ] HTTPS/TLS 1.2+ enforced
- [ ] Firewall rules configured
- [ ] VNet integration (if applicable)
- [ ] DDoS protection enabled
- [ ] WAF rules configured
- [ ] SSL certificates auto-renewed

## Authentication & Authorization
- [ ] No default credentials
- [ ] MFA enabled for Azure portal
- [ ] Service Principal with minimal permissions
- [ ] RBAC properly configured
- [ ] API keys rotated monthly
- [ ] OAuth2/OIDC configured
- [ ] JWT tokens with short expiration

## Data Protection
- [ ] All secrets in Key Vault
- [ ] Database encryption at rest
- [ ] Database backups automated
- [ ] Connection strings use Managed Identity
- [ ] No credentials in code/config
- [ ] Sensitive data masked in logs
- [ ] GDPR compliance verified

## Application Security
- [ ] OWASP Top 10 mitigated
- [ ] Input validation on all endpoints
- [ ] SQL injection prevention
- [ ] XSS protection
- [ ] CSRF tokens
- [ ] Rate limiting enabled
- [ ] Security headers configured

## API Security
- [ ] API authentication required
- [ ] API versioning implemented
- [ ] Error messages don't leak info
- [ ] Deprecated endpoints removed
- [ ] API documentation secure

## Monitoring & Logging
- [ ] Application Insights enabled
- [ ] Centralized logging configured
- [ ] Security audit logs enabled
- [ ] Alerts for suspicious activity
- [ ] Incident response plan
- [ ] Regular security reviews

## Deployment Security
- [ ] Secrets not in git history
- [ ] Deployment approved by security
- [ ] Infrastructure as Code reviewed
- [ ] Dependency vulnerabilities scanned
- [ ] Container images scanned
- [ ] Post-deployment security test

## Compliance
- [ ] GDPR compliance
- [ ] Data residency met
- [ ] Audit logs retained
- [ ] Compliance documentation
- [ ] Regular security assessments

## Incident Response
- [ ] Incident response plan documented
- [ ] On-call rotation established
- [ ] Runbooks for common issues
- [ ] Backup & restore tested
- [ ] RTO/RPO defined
Security Commands:

Bash

# Enable Azure Defender
az security auto-provisioning-setting update \
  --resource-group techtwheel-rg \
  --auto-provision "On"

# Enable SQL threat detection
az sql server threat-detection-policy update \
  --resource-group techtwheel-rg \
  --server techtwheel-server \
  --state Enabled

# Enable Azure Key Vault logging
az monitor diagnostic-settings create \
  --resource-group techtwheel-rg \
  --resource techtwheel-vault \
  --resource-type "Microsoft.KeyVault/vaults" \
  --name "KeyVaultDiagnostics" \
  --logs '[{"category":"AuditEvent","enabled":true}]'

# Run security scan
az webapp identity assign \
  --resource-group techtwheel-rg \
  --name techtwheel-api

# Verify HTTPS
az webapp update \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --https-only true
Checklist:

 All security items reviewed
 Issues documented and fixed
 Security headers added
 Rate limiting active
 Monitoring alerts working
 Incident response plan ready
Git Commit:

Bash

git add .
git commit -m "Week7-Day5-Morning: Production security hardening"
git push origin main
EVENING (9-10 PM): Blog Post & Week Summary
Write Blog Post: "Deploying Enterprise .NET Apps to Azure"

Sections:

Azure Architecture Overview (200 words)
Resource Group Organization (150 words)
App Service Deployment (200 words)
Database Setup (150 words)
Securing with Key Vault & Managed Identity (200 words)
Real-time with Service Bus (150 words)
Monitoring with Application Insights (150 words)
Production Security Checklist (200 words)
Cost Optimization (100 words)
Code Examples (300 words)
Publish on: Medium or Dev.to

Weekly Summary Document:

Create file: Week7/WEEK7-SUMMARY.md

Markdown

# Week 7 Summary: Azure Cloud Deployment

## What I Built
✅ Azure resource group with organized resources
✅ App Service deployment with staging slots
✅ Azure SQL Database with geo-replication
✅ Managed Identity for passwordless auth
✅ Azure Service Bus for async messaging
✅ Redis Cache for performance
✅ Key Vault for centralized secrets
✅ Application Insights monitoring
✅ Security hardening

## Key Learnings

### Resource Management
- Azure Resource Groups organize related resources
- Naming conventions help with at-scale management
- Cost management is critical (set budgets!)

### Deployment Strategy
- Staging slots enable zero-downtime deployments
- Managed Identity eliminates password management
- Auto-scaling handles traffic spikes

### Data & Messaging
- Azure Service Bus replaces on-premises RabbitMQ
- Geo-replication provides high availability
- Connection strings no longer in config (Key Vault)

### Monitoring
- Application Insights gives real-time visibility
- Custom metrics help understand business impact
- Alert rules enable proactive responses

## Real Numbers at Production Scale

| Metric | Value |
|--------|-------|
| Concurrent Users | 10,000+ |
| Request Latency | <200ms |
| Error Rate | <0.1% |
| Uptime | 99.95% |
| Monthly Cost | $500-700 |

## Architecture Diagram
[Users]
↓
[CDN]
↓
[Azure Front Door - Load Balancer]
↓
[App Service (3 instances with auto-scale)]
↓
[Azure SQL + Geo-replica]
[Redis Cache]
[Service Bus (async)]
[Key Vault (secrets)]
[Application Insights (monitoring)]

text


## Challenges & Solutions

### Challenge 1: Cost Management
- Solution: Azure Cost Management alerts, Reserved Instances

### Challenge 2: Zero-downtime Deployments
- Solution: Deployment slots with swap

### Challenge 3: Secret Management
- Solution: Key Vault + Managed Identity

## Next Week

Week 8 will focus on:
- CI/CD pipelines with GitHub Actions
- Automated testing
- Security scanning
- Performance optimization
Checklist:

 Blog post written and published
 Week 7 summary documented
 All code committed to GitHub
 Azure resources tagged and organized
 Cost monitoring enabled
 Security audit complete
Git Commit:

Bash

git add .
git commit -m "Week7 Complete: Enterprise Azure deployment ready for production"
git push origin main
Week 7 Summary:

text

✅ WEEK 7 COMPLETE!

What You Built:
✓ Complete Azure infrastructure
✓ App Service with auto-scaling
✓ Azure SQL Database with backups
✓ Managed Identity authentication
✓ Service Bus for async messaging
✓ Redis Cache integration
✓ Key Vault for secrets
✓ Application Insights monitoring
✓ Security hardening

Metrics:
- Hours: 15/15 ✓
- Commits: 12+
- Blog posts: 7/12 ✓
- Azure resources: 8+

Skills Improved:
- Azure: 1/5 → 3.5/5
- DevOps: 0/5 → 2.5/5
- Security: 3/5 → 4/5

System now production-ready!
Next Week: CI/CD & Monitoring
WEEK 8: CI/CD & PRODUCTION OBSERVABILITY
Nov 16-22, 2026 | Goal: Automated deployments and monitoring at scale
MONDAY, NOV 16
MORNING (8-10 AM): GitHub Actions CI/CD Pipelines
Video Search:

text

YouTube Search 1: "GitHub Actions CI/CD pipeline tutorial"
YouTube Search 2: "Deploy .NET to Azure with GitHub Actions"

Duration: 45-50 minutes
Creator: Code Maze or GitHub official

Topics:
- Workflow files (.yml)
- Build matrix for multiple versions
- Secrets management
- Artifacts and caching
- Status checks and reviews
Coding Task: Create CI/CD Pipeline

Create file: .github/workflows/deploy.yml

YAML

name: Build and Deploy

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  AZURE_WEBAPP_NAME: techtwheel-api
  AZURE_WEBAPP_SLOT: production
  DOTNET_VERSION: '8.0.x'

jobs:
  # Job 1: Build and Test
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0  # Full history for versioning
    
    # Cache dependencies
    - name: Cache NuGet packages
      uses: actions/cache@v3
      with:
        path: ~/.nuget/packages
        key: ${{ runner.os }}-nuget-${{ hashFiles('**/packages.lock.json') }}
        restore-keys: |
          ${{ runner.os }}-nuget-
    
    # Setup .NET
    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: ${{ env.DOTNET_VERSION }}
    
    # Install dependencies
    - name: Restore dependencies
      run: dotnet restore
    
    # Build
    - name: Build
      run: dotnet build --configuration Release --no-restore
    
    # Run tests
    - name: Run unit tests
      run: dotnet test --configuration Release --no-build --verbosity normal --logger "trx;LogFileName=test-results.trx"
    
    # Code coverage
    - name: Measure code coverage
      run: dotnet test --configuration Release --no-build /p:CollectCoverage=true /p:CoverageFormat=cobertura
    
    # Upload coverage to Codecov
    - name: Upload coverage reports
      uses: codecov/codecov-action@v3
      with:
        files: ./coverage.cobertura.xml
        flags: unittests
        name: codecov-umbrella
    
    # Security scanning with CodeQL
    - name: Initialize CodeQL
      uses: github/codeql-action/init@v2
      with:
        languages: 'csharp'
    
    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v2
    
    # Publish build artifacts
    - name: Publish artifacts
      run: dotnet publish --configuration Release --output ${{ github.workspace }}/publish
    
    - name: Upload build artifacts
      uses: actions/upload-artifact@v3
      with:
        name: published-app
        path: ${{ github.workspace }}/publish
        retention-days: 5

  # Job 2: Dependency Scanning
  security:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Scan NuGet dependencies
      uses: gitcoindev/github-nuget-nugetaudit-action@v1
      with:
        project: './TechTweet.sln'

  # Job 3: Docker Build (only on main)
  docker:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    needs: build
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Download artifacts
      uses: actions/download-artifact@v3
      with:
        name: published-app
        path: ./publish
    
    - name: Login to Azure Container Registry
      uses: azure/docker-login@v1
      with:
        login-server: techtwheel.azurecr.io
        username: ${{ secrets.AZURE_REGISTRY_USERNAME }}
        password: ${{ secrets.AZURE_REGISTRY_PASSWORD }}
    
    - name: Build and push Docker image
      uses: docker/build-push-action@v4
      with:
        context: .
        push: true
        tags: |
          techtwheel.azurecr.io/techtwheel-api:${{ github.sha }}
          techtwheel.azurecr.io/techtwheel-api:latest
        build-args: |
          BUILD_VERSION=${{ github.sha }}

  # Job 4: Deploy to Azure (only on main)
  deploy:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    needs: [build, security, docker]
    environment: production
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Download artifacts
      uses: actions/download-artifact@v3
      with:
        name: published-app
        path: ./publish
    
    - name: Login to Azure
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - name: Deploy to Azure App Service (Staging)
      uses: azure/webapps-deploy@v2
      with:
        app-name: ${{ env.AZURE_WEBAPP_NAME }}
        slot-name: staging
        package: ./publish
    
    - name: Run smoke tests on staging
      run: |
        for i in {1..30}; do
          if curl -f https://techtwheel-api-staging.azurewebsites.net/health/live; then
            echo "Staging health check passed"
            break
          fi
          echo "Attempt $i failed, retrying..."
          sleep 10
        done
    
    - name: Swap staging to production
      run: |
        az webapp deployment slot swap \
          --resource-group techtwheel-rg \
          --name techtwheel-api \
          --slot staging
    
    - name: Run smoke tests on production
      run: |
        curl -f https://techtwheel-api.azurewebsites.net/health/live
    
    - name: Notify deployment success
      if: success()
      uses: actions/github-script@v6
      with:
        script: |
          github.rest.issues.createComment({
            issue_number: context.issue.number,
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: '✅ Deployment successful to production!'
          })
    
    - name: Notify deployment failure
      if: failure()
      uses: actions/github-script@v6
      with:
        script: |
          github.rest.issues.createComment({
            issue_number: context.issue.number,
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: '❌ Deployment failed! Check logs.'
          })

  # Job 5: Performance Testing (after production deployment)
  performance:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    needs: deploy
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Run k6 performance tests
      run: |
        docker run --rm -v ${{ github.workspace }}:/scripts \
          grafana/k6 run /scripts/performance-tests.js \
          --vus 100 \
          --duration 30s
Setup GitHub Secrets:

Bash

# In GitHub repo settings → Secrets and variables → Actions

# Azure credentials (for deployment)
AZURE_CREDENTIALS: 
(output of: az ad sp create-for-rbac --role contributor --scopes /subscriptions/YOUR_SUBSCRIPTION)

# Azure Container Registry
AZURE_REGISTRY_USERNAME: (username)
AZURE_REGISTRY_PASSWORD: (password)

# Database password
DB_PASSWORD: (from Key Vault)

# API Keys
JWT_SECRET: (from Key Vault)
Example GitHub Secret Value (AZURE_CREDENTIALS):

JSON

{
  "clientId": "00000000-0000-0000-0000-000000000000",
  "clientSecret": "your-client-secret",
  "subscriptionId": "00000000-0000-0000-0000-000000000000",
  "tenantId": "00000000-0000-0000-0000-000000000000"
}
Create Dockerfile for CI/CD:

Create file: Dockerfile

Dockerfile

# Stage 1: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY ["TechTweet.sln", "."]
COPY ["TechTweet.API/TechTweet.API.csproj", "TechTweet.API/"]
COPY ["TechTweet.Domain/TechTweet.Domain.csproj", "TechTweet.Domain/"]
COPY ["TechTweet.Infrastructure/TechTweet.Infrastructure.csproj", "TechTweet.Infrastructure/"]
COPY ["TechTweet.Tests/TechTweet.Tests.csproj", "TechTweet.Tests/"]

RUN dotnet restore "TechTweet.sln"

COPY . .
WORKDIR "/src/TechTweet.API"

RUN dotnet build "TechTweet.API.csproj" -c Release -o /app/build

# Stage 2: Publish
FROM build AS publish
RUN dotnet publish "TechTweet.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Stage 3: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .

EXPOSE 5000
HEALTHCHECK --interval=30s --timeout=3s --start-period=40s --retries=3 \
    CMD curl -f http://localhost:5000/health/live || exit 1

ENTRYPOINT ["dotnet", "TechTweet.API.dll"]
Checklist:

 GitHub Actions workflow created
 All jobs configured (build, test, security, docker, deploy)
 GitHub secrets added
 Dockerfile ready
 Can trigger pipeline via push to main
 Pipeline runs successfully
Git Commit:

Bash

git add .github/workflows/deploy.yml Dockerfile
git commit -m "Week8-Day1-Morning: GitHub Actions CI/CD pipeline"
git push origin main
EVENING (9-10 PM): Advanced Pipeline Features
Coding Task: Add Quality Gates & Approvals

Create file: .github/workflows/quality-gates.yml

YAML

name: Quality Gates

on: pull_request

jobs:
  quality-checks:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: '8.0.x'
    
    # Code style check
    - name: Check code style
      run: dotnet format --verify-no-changes
    
    # Static analysis
    - name: Run Roslyn analyzers
      run: dotnet build -p:EnforceCodeStyleInBuild=true
    
    # Test coverage threshold
    - name: Check test coverage
      run: |
        dotnet test --configuration Release /p:CollectCoverage=true
        if [ "$(grep -oP 'coverage[">]\K[^"]*' coverage.xml | head -1)" -lt "80" ]; then
          echo "❌ Code coverage below 80%"
          exit 1
        fi
    
    # Dependency vulnerabilities
    - name: Check vulnerable dependencies
      run: dotnet list package --vulnerable
    
    - name: Create PR comment with status
      if: always()
      uses: actions/github-script@v6
      with:
        script: |
          const status = '${{ job.status }}';
          const comment = status === 'success' 
            ? '✅ All quality gates passed!' 
            : '❌ Quality gates failed. Review the logs.';
          github.rest.issues.createComment({
            issue_number: context.issue.number,
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: comment
          });
Create Status Check Rules (in GitHub repo settings):

text

Branch protection rules for 'main':
- Require pull request reviews before merging: 1
- Require status checks to pass: 
  ✓ build (must pass)
  ✓ security (must pass)
  ✓ quality-checks (must pass)
- Require branches to be up to date before merging
- Require code owner reviews
- Require approval of the latest reviewable commit
Checklist:

 Quality gates workflow created
 Branch protection rules configured
 Code style enforcement enabled
 Test coverage enforced (80%+ required)
 Dependency scanning active
 PR requires approvals before merge
Git Commit:

Bash

git add .github/workflows/quality-gates.yml
git commit -m "Week8-Day1-Evening: Quality gates and branch protection"
git push origin main
TUESDAY, NOV 17
MORNING (8-10 AM): Load Testing & Performance Baselines
Video Search:

text

YouTube Search: "k6 load testing tutorial for APIs"

Duration: 40-45 minutes

Topics:
- Virtual users (VUs)
- Ramp-up strategies
- Metrics (response time, error rate, throughput)
- Thresholds and pass/fail criteria
Coding Task: Create Performance Tests

Create file: performance-tests.js (k6 script)

JavaScript

import http from 'k6/http';
import { check, group, sleep } from 'k6';
import { Rate, Trend, Counter, Gauge } from 'k6/metrics';

// Custom metrics
const errorRate = new Rate('errors');
const apiDuration = new Trend('api_duration');
const successfulRequests = new Counter('successful_requests');
const concurrentUsers = new Gauge('concurrent_users');

// Test configuration
export const options = {
  scenarios: {
    // Scenario 1: Gradual ramp-up
    ramping: {
      executor: 'ramping-vus',
      startVUs: 0,
      stages: [
        { duration: '2m', target: 100 },   // Ramp to 100 users
        { duration: '5m', target: 100 },   // Stay at 100
        { duration: '2m', target: 200 },   // Ramp to 200
        { duration: '5m', target: 200 },   // Stay at 200
        { duration: '2m', target: 0 },     // Ramp down
      ],
    },
    // Scenario 2: Spike test
    spike: {
      executor: 'ramping-vus',
      startVUs: 10,
      stages: [
        { duration: '1m', target: 100 },
        { duration: '2m', target: 1000 },  // Spike!
        { duration: '2m', target: 100 },
        { duration: '1m', target: 0 },
      ],
      gracefulStop: '30s',
    },
  },
  
  // Thresholds - defines pass/fail criteria
  thresholds: {
    // 95% of requests must complete in < 500ms
    'http_req_duration': ['p(95)<500', 'p(99)<1000'],
    // Error rate must be < 1%
    'errors': ['rate<0.01'],
    // 99% of requests must succeed
    'http_req_failed': ['rate<0.01'],
  },
  
  // System requirements
  systemTags: ['scenario:ramping', 'env:production'],
};

const BASE_URL = 'https://techtwheel-api.azurewebsites.net';
const AUTH_TOKEN = 'your-jwt-token'; // Or use env variable

export default function () {
  const headers = {
    'Authorization': `Bearer ${AUTH_TOKEN}`,
    'Content-Type': 'application/json',
  };
  
  // ============= SCENARIO 1: User Registration Flow =============
  group('User Registration', () => {
    const registrationData = {
      username: `user_${__VU}_${__ITER}@techtwheel.com`,
      email: `user_${__VU}_${__ITER}@techtwheel.com`,
      password: 'SecurePass123!',
    };
    
    let registerRes = http.post(
      `${BASE_URL}/api/auth/register`,
      JSON.stringify(registrationData),
      { headers }
    );
    
    check(registerRes, {
      'registration successful': (r) => r.status === 201,
      'token received': (r) => r.json('token') !== undefined,
    });
    
    errorRate.add(registerRes.status !== 201);
    apiDuration.add(registerRes.timings.duration);
    if (registerRes.status === 201) successfulRequests.add(1);
  });
  
  sleep(1);
  
  // ============= SCENARIO 2: Create Tweet =============
  group('Create Tweet', () => {
    const tweetData = {
      content: `Test tweet from VU ${__VU} iteration ${__ITER}`,
    };
    
    let tweetRes = http.post(
      `${BASE_URL}/api/tweets`,
      JSON.stringify(tweetData),
      { headers }
    );
    
    check(tweetRes, {
      'tweet created': (r) => r.status === 201,
      'response time < 300ms': (r) => r.timings.duration < 300,
    });
    
    errorRate.add(tweetRes.status !== 201);
    apiDuration.add(tweetRes.timings.duration);
    
    if (tweetRes.status === 201) {
      const tweetId = tweetRes.json('id');
      return tweetId;
    }
  });
  
  sleep(2);
  
  // ============= SCENARIO 3: Get Feed =============
  group('Get User Feed', () => {
    let feedRes = http.get(
      `${BASE_URL}/api/feed/home`,
      { headers }
    );
    
    check(feedRes, {
      'feed retrieved': (r) => r.status === 200,
      'tweets in feed': (r) => r.json('tweets').length > 0,
      'response time < 500ms': (r) => r.timings.duration < 500,
    });
    
    errorRate.add(feedRes.status !== 200);
    apiDuration.add(feedRes.timings.duration);
    successfulRequests.add(feedRes.status === 200 ? 1 : 0);
  });
  
  sleep(1);
  
  // ============= SCENARIO 4: Like Tweet =============
  group('Like Tweet', () => {
    const tweetId = 123; // In real scenario, use from previous response
    
    let likeRes = http.post(
      `${BASE_URL}/api/tweets/${tweetId}/like`,
      null,
      { headers }
    );
    
    check(likeRes, {
      'like successful': (r) => r.status === 200,
      'response time < 200ms': (r) => r.timings.duration < 200,
    });
    
    errorRate.add(likeRes.status !== 200);
    apiDuration.add(likeRes.timings.duration);
  });
  
  sleep(1);
  
  // ============= SCENARIO 5: Search Tweets =============
  group('Search Tweets', () => {
    let searchRes = http.get(
      `${BASE_URL}/api/tweets/search?q=technology`,
      { headers }
    );
    
    check(searchRes, {
      'search successful': (r) => r.status === 200,
      'results returned': (r) => r.json('results').length > 0,
      'response time < 600ms': (r) => r.timings.duration < 600,
    });
    
    errorRate.add(searchRes.status !== 200);
    apiDuration.add(searchRes.timings.duration);
  });
  
  // Update concurrent users gauge
  concurrentUsers.add(__VU);
  
  sleep(Math.random() * 3);
}

// Custom summary
export function handleSummary(data) {
  return {
    'stdout': textSummary(data, { indent: ' ', enableColors: true }),
    'summary.json': JSON.stringify(data),
  };
}
Run Load Test:

Bash

# Install k6: https://k6.io/docs/getting-started/installation/

# Run ramping scenario
k6 run performance-tests.js --scenario ramping

# Run spike scenario
k6 run performance-tests.js --scenario spike

# Run with specific VUs and duration
k6 run performance-tests.js --vus 100 --duration 30s

# With thresholds (exit with error if not met)
k6 run performance-tests.js --threshold 'http_req_duration{staticAsset:yes}<1000'
Expected Output:

text

     data_received..................: 1.2 MB  1.2 kB/s
     data_sent.......................: 523 kB  523 B/s
     http_req_blocked................: avg=2.35ms   min=1.21ms   med=1.98ms   max=15.63ms   p(90)=4.53ms   p(95)=6.12ms
     http_req_connecting.............: avg=1.12ms   min=0s       med=0s       max=8.32ms   p(90)=2.45ms   p(95)=3.56ms
     http_req_duration...............: avg=157.69ms min=88.12ms  med=142.31ms max=1.12s     p(90)=248.41ms p(95)=315.23ms
     http_req_failed.................: 0.00%
     http_req_receiving..............: avg=5.23ms   min=1.25ms   med=4.56ms   max=95.32ms  p(90)=9.23ms   p(95)=12.11ms
     http_req_sending................: avg=1.12ms   min=0.32ms   med=0.98ms   max=8.23ms   p(90)=1.95ms   p(95)=2.34ms
     http_req_tls_handshaking.......: avg=0ms      min=0s       med=0s       max=0s       p(90)=0s       p(95)=0s
     http_req_waiting................: avg=151.34ms min=82.34ms  med=136.43ms max=1.05s    p(90)=240.12ms p(95)=305.34ms
     http_reqs........................: 5000    5.23/s
     iteration_duration..............: avg=6.35s    min=5.01s    med=6.21s    max=12.34s   p(90)=7.45s    p(95)=8.56s
     iterations........................: 5000    5.23
     vus..............................: 1       min=1     max=200
     vus_max...........................: 200     min=200   max=200

✓ p(95)<500
✓ p(99)<1000
✓ errors rate<0.01
✓ http_req_failed rate<0.01

SUCCESS: All thresholds met! ✅
Checklist:

 k6 installed and working
 performance-tests.js created
 Can run load tests
 Thresholds configured
 Response times acceptable (<500ms p95)
 Error rate < 1%
Git Commit:

Bash

git add performance-tests.js
git commit -m "Week8-Day2-Morning: k6 load testing suite"
git push origin main
EVENING (9-10 PM): Continuous Performance Monitoring
Coding Task: Add Performance Baselines to CI

Create file: .github/workflows/performance-check.yml

YAML

name: Performance Check

on:
  push:
    branches: [main]
  schedule:
    - cron: '0 2 * * *'  # Daily at 2 AM

jobs:
  performance-test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup k6
      run: |
        sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3232A
        echo "deb https://dl.k6.io/deb stable main" | sudo tee /etc/apt/sources.list.d/k6-stable.list
        sudo apt-get update
        sudo apt-get install k6
    
    - name: Run performance tests
      run: |
        k6 run performance-tests.js \
          --vus 100 \
          --duration 5m \
          --out json=results.json
      timeout-minutes: 10
    
    - name: Parse results
      run: |
        # Extract metrics
        export P95=$(jq '.metrics.http_req_duration[0].stats.p95' results.json)
        export ERROR_RATE=$(jq '.metrics.http_req_failed[0].stats.value' results.json)
        
        echo "P95 Response Time: ${P95}ms"
        echo "Error Rate: ${ERROR_RATE}%"
        
        # Fail if below thresholds
        if (( $(echo "$P95 > 500" | bc -l) )); then
          echo "❌ P95 response time exceeded 500ms"
          exit 1
        fi
        
        if (( $(echo "$ERROR_RATE > 0.01" | bc -l) )); then
          echo "❌ Error rate exceeded 1%"
          exit 1
        fi
        
        echo "✅ Performance check passed"
    
    - name: Upload results
      if: always()
      uses: actions/upload-artifact@v3
      with:
        name: perf-results
        path: results.json
Checklist:

 Performance test workflow created
 Can run automatically on schedule
 Metrics tracked over time
 Fails if thresholds exceeded
 Results stored for analysis
Git Commit:

Bash

git add .github/workflows/performance-check.yml
git commit -m "Week8-Day2-Evening: Continuous performance monitoring"
git push origin main
WEDNESDAY, NOV 18
MORNING (8-10 AM): Advanced Monitoring & Log Analytics
Video Search:

text

YouTube Search: "Azure Application Insights advanced monitoring"

Duration: 40-45 minutes

Topics:
- Custom metrics and events
- Log Analytics KQL queries
- Distributed tracing
- Alerting strategies
Coding Task: Advanced Application Insights

Create file: Infrastructure/MonitoringService.cs

csharp

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using System.Diagnostics;

public interface IMonitoringService
{
    IDisposable StartOperation(string operationName);
    void TrackCustomMetric(string metricName, double value, Dictionary<string, string> properties = null);
    void TrackException(Exception ex, SeverityLevel severity = SeverityLevel.Error);
    void TrackDependency(string dependencyType, string dependencyName, string commandName, DateTime startTime, TimeSpan duration, bool success);
}

public class ApplicationInsightsMonitoringService : IMonitoringService
{
    private readonly TelemetryClient _telemetryClient;
    private readonly ILogger<ApplicationInsightsMonitoringService> _logger;
    
    public ApplicationInsightsMonitoringService(
        TelemetryClient telemetryClient,
        ILogger<ApplicationInsightsMonitoringService> logger)
    {
        _telemetryClient = telemetryClient;
        _logger = logger;
    }
    
    /// <summary>
    /// Tracks a business operation end-to-end
    /// </summary>
    public IDisposable StartOperation(string operationName)
    {
        var operation = _telemetryClient.StartOperation<RequestTelemetry>(operationName);
        _logger.LogInformation($"Operation started: {operationName}");
        return operation;
    }
    
    /// <summary>
    /// Track custom metrics (business KPIs)
    /// </summary>
    public void TrackCustomMetric(string metricName, double value, Dictionary<string, string> properties = null)
    {
        var metrics = new Dictionary<string, double>
        {
            { metricName, value }
        };
        
        _telemetryClient.TrackEvent(
            eventName: $"CustomMetric_{metricName}",
            properties: properties,
            metrics: metrics
        );
        
        _logger.LogInformation($"Metric tracked: {metricName} = {value}");
    }
    
    /// <summary>
    /// Track exceptions with context
    /// </summary>
    public void TrackException(Exception ex, SeverityLevel severity = SeverityLevel.Error)
    {
        var exceptionTelemetry = new ExceptionTelemetry(ex)
        {
            SeverityLevel = severity
        };
        
        exceptionTelemetry.Properties.Add("Timestamp", DateTime.UtcNow.ToString("O"));
        exceptionTelemetry.Properties.Add("MachineName", Environment.MachineName);
        
        _telemetryClient.TrackException(exceptionTelemetry);
        _logger.LogError(ex, "Exception tracked");
    }
    
    /// <summary>
    /// Track external dependencies (DB, API calls)
    /// </summary>
    public void TrackDependency(
        string dependencyType,
        string dependencyName,
        string commandName,
        DateTime startTime,
        TimeSpan duration,
        bool success)
    {
        var dependencyTelemetry = new DependencyTelemetry
        {
            Type = dependencyType,
            Name = dependencyName,
            Data = commandName,
            Timestamp = startTime,
            Duration = duration,
            Success = success
        };
        
        _telemetryClient.TrackDependency(dependencyTelemetry);
    }
}

// Usage in service
public class TweetService
{
    private readonly IMonitoringService _monitoring;
    private readonly IRepository<Tweet> _repository;
    
    public TweetService(
        IMonitoringService monitoring,
        IRepository<Tweet> repository)
    {
        _monitoring = monitoring;
        _repository = repository;
    }
    
    public async Task<Tweet> CreateTweetAsync(long userId, string content)
    {
        // Track operation
        using (var operation = _monitoring.StartOperation("CreateTweet"))
        {
            try
            {
                var stopwatch = Stopwatch.StartNew();
                
                // Validation
                if (string.IsNullOrEmpty(content) || content.Length > 280)
                    throw new ArgumentException("Invalid content");
                
                // Create tweet
                var tweet = new Tweet
                {
                    UserId = userId,
                    Content = content,
                    CreatedAt = DateTime.UtcNow
                };
                
                // Save to DB
                await _repository.AddAsync(tweet);
                stopwatch.Stop();
                
                // Track custom metrics
                _monitoring.TrackCustomMetric(
                    "TweetCreationTime",
                    stopwatch.ElapsedMilliseconds,
                    new Dictionary<string, string>
                    {
                        { "UserId", userId.ToString() },
                        { "ContentLength", content.Length.ToString() }
                    }
                );
                
                // Track this specific metric in business context
                _monitoring.TrackCustomMetric(
                    "DailyTweetsCreated",
                    1,
                    new Dictionary<string, string>
                    {
                        { "Date", DateTime.UtcNow.Date.ToString("yyyy-MM-dd") }
                    }
                );
                
                return tweet;
            }
            catch (Exception ex)
            {
                _monitoring.TrackException(ex);
                throw;
            }
        }
    }
}
Create KQL Queries for Log Analytics:

Create file: Monitoring/KQLQueries.md

Markdown

# Log Analytics KQL Queries for TechTweet

## Query 1: Average Response Time by Endpoint
```kusto
customMetrics
| where name == "http_req_duration"
| summarize AvgDuration=avg(value) by endpoint=tostring(customDimensions.endpoint)
| render columnchart
Query 2: Error Rate Over Time
kusto

requests
| where timestamp > ago(24h)
| summarize FailureCount=sum(itemCount) by bin(timestamp, 5m), success
| render areachart
Query 3: Dependency Failures (Database, APIs)
kusto

dependencies
| where success == false
| summarize FailureCount=count() by type, name, resultCode
| order by FailureCount desc
Query 4: Slow Transactions (> 500ms)
kusto

customEvents
| where name == "CustomMetric_TweetCreationTime"
| where todouble(customMeasurements.value) > 500
| project timestamp, UserId=customDimensions.UserId, Duration=customMeasurements.value
| order by Duration desc
Query 5: Exception Trends
kusto

exceptions
| where timestamp > ago(7d)
| summarize ExceptionCount=count() by problemId, bin(timestamp, 1d)
| render timechart
Query 6: Database Query Performance
kusto

dependencies
| where type == "SQL"
| summarize 
    Count=count(), 
    AvgDuration=avg(duration),
    MaxDuration=max(duration),
    FailureRate=todouble(countif(success==false))/count()*100
    by name
| order by MaxDuration desc
Query 7: User Activity Heatmap
kusto

customEvents
| where name == "CustomMetric_DailyTweetsCreated"
| extend Date=todate(customDimensions.Date)
| summarize TweetsCount=sum(todouble(customMeasurements.value)) by Date
| render columnchart
Query 8: Top 10 Failing Endpoints
kusto

requests
| where success == false
| summarize FailureCount=count() by name
| top 10 by FailureCount
| render barchart
Query 9: P95, P99 Response Times
kusto

requests
| where timestamp > ago(1d)
| summarize
    P50=percentile(duration, 50),
    P95=percentile(duration, 95),
    P99=percentile(duration, 99),
    Max=max(duration)
    by name
Query 10: Memory & CPU Usage Trends
kusto

performanceCounters
| where ((category == "Process" and counter == "% Processor Time") 
         or (category == "Memory" and counter == "Available Bytes"))
| project timestamp, counter, value
| render timechart
text


**Create Alert Rules:**

```bash
# Alert: P95 response time > 500ms
az monitor metrics alert create \
  --resource-group techtwheel-rg \
  --scopes /subscriptions/xxx/resourceGroups/techtwheel-rg/providers/microsoft.insights/components/techtwheel-insights \
  --name "HighLatency-P95" \
  --condition "avg http_req_duration > 500" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --severity 2

# Alert: Error rate > 1%
az monitor metrics alert create \
  --resource-group techtwheel-rg \
  --scopes /subscriptions/xxx/resourceGroups/techtwheel-rg/providers/microsoft.insights/components/techtwheel-insights \
  --name "HighErrorRate" \
  --condition "avg http_req_failed > 1" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --severity 1

# Alert: Exception spike
az monitor metrics alert create \
  --resource-group techtwheel-rg \
  --scopes /subscriptions/xxx/resourceGroups/techtwheel-rg/providers/microsoft.insights/components/techtwheel-insights \
  --name "ExceptionSpike" \
  --condition "count exceptions > 20" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --severity 2
Checklist:

 Custom metrics implemented
 KQL queries created
 Alert rules configured
 Can query logs in Azure Portal
 Dashboard created
Git Commit:

Bash

git add Infrastructure/MonitoringService.cs Monitoring/KQLQueries.md
git commit -m "Week8-Day3-Morning: Advanced Application Insights monitoring"
git push origin main
EVENING (9-10 PM): Incident Response & Runbooks
Coding Task: Create Incident Runbooks

Create file: Operations/IncidentRunbooks.md

Markdown

# Incident Response Runbooks for TechTweet

## Runbook 1: API Latency Spike (P95 > 1 second)

### Detection
- Alert: "HighLatency-P95" triggered
- Timeline: Last 5 minutes showing spike

### Investigation (5 min)
```kusto
-- Step 1: Identify affected endpoints
requests
| where timestamp > ago(10m)
| where duration > 1000
| summarize Count=count() by name, success
| order by Count desc

-- Step 2: Check for errors
exceptions
| where timestamp > ago(10m)
| summarize ErrorCount=count() by type

-- Step 3: Database performance
dependencies
| where type == "SQL"
| where timestamp > ago(10m)
| summarize AvgDuration=avg(duration) by name
| order by AvgDuration desc
Response (10-15 min)
Check Database:

text

SELECT * FROM sys.dm_exec_requests;
SELECT * FROM sys.dm_exec_sessions;
Look for long-running queries
Check for locks
Check Application:

Review recent deployments (in last 1 hr)
Check Application Insights metrics
Immediate Actions:

If DB issue: Kill long queries OR increase resources
If app issue: Restart App Service
If external API: Check third-party status pages
Escalation:

If unresolved in 10 min: Contact DevOps
If in prod: Notify team lead
Resolution
text

Document:
- Root cause
- Time to resolution
- Permanent fix (if needed)
- Prevention measures
Runbook 2: High Error Rate (> 5%)
Detection
Alert: "HighErrorRate" triggered
Error types to investigate: 500, 503, 429
Investigation (5 min)
kusto

requests
| where timestamp > ago(15m)
| where success == false
| summarize Count=count() by resultCode, name
| order by Count desc

exceptions
| where timestamp > ago(15m)
| summarize Count=count() by type, problemId
| top 10 by Count
Response
Check for deploys (could be recent bad deployment)
Check databases (connection issues, deadlocks)
Check external services (payment gateway, email, APIs)
Check infrastructure (disk space, memory, CPU)
Immediate Actions
If bad deploy: Revert to previous version
If DB issue: Restart SQL Server OR add replicas
If external: Switch fallback OR disable feature
Scale up if CPU/Memory high
Runbook 3: Database Connection Pool Exhaustion
Symptoms
Requests timeout
"Connection timeout" errors
Investigation
SQL

-- Check connection count
SELECT COUNT(*) FROM sys.dm_exec_sessions;

-- Check blocking
SELECT * FROM sys.dm_exec_requests WHERE status = 'suspended';

-- Check wait stats
SELECT TOP 10 * FROM sys.dm_exec_session_wait_stats 
WHERE wait_duration_ms > 0;
Resolution
Increase connection pool size in connection string
Check for connection leaks (not disposing connections)
Reduce long-running queries
Add read replicas if queries are slow
Runbook 4: Unhandled Exception Spike
Detection
Exceptions count > 50 in 5 minutes
Specific error type repeating
Immediate Debug
kusto

exceptions
| where timestamp > ago(20m)
| summarize Count=count() by type
| order by Count desc
| top 5

-- Drill into specific exception
exceptions
| where type == "NullReferenceException"
| project timestamp, details, stackTrace, customDimensions
Common Causes & Fixes
NullReferenceException → Null check missing (code defect)
TimeoutException → External API slow (increase timeout)
OutOfMemoryException → Memory leak (restart app)
SqlException → DB connection issue (restart DB)
Runbook 5: Deployment Failed
Detection
Deployment alert triggered
Health checks failing after deployment
Rollback (Immediate)
Bash

# In Azure DevOps or Portal
az webapp deployment slot swap \
  --resource-group techtwheel-rg \
  --name techtwheel-api \
  --slot staging \
  --action swap-only  # Stays in staging
  
# Wait for it to stabilize
# Then swap back to production
Investigation
text

1. Check deployment logs (Azure DevOps)
2. Check app startup errors (App Service logs)
3. Check if secrets/config missing (Key Vault)
4. Check database migrations (if applicable)
5. Check dependency versions (.NET, NuGet packages)
Prevention
Run smoke tests before swapping to production
Have automated rollback on health check failure
Test deployment in staging first
Runbook 6: Security Incident (SQL Injection Attempt Detected)
Immediate Actions (< 5 min)
text

1. Check audit logs for suspicious queries
2. Block attacker IP (add to firewall)
3. Review application logs for anomalies
4. Check if data was extracted (sensitive columns)
Investigation
SQL

-- Check SQL logs
SELECT * FROM sys.server_audit_session_details 
WHERE create_date > GETUTCDATE() - 1;

-- Check for unusual queries
SELECT statement FROM sys.dm_exec_requests 
WHERE command NOT IN ('SELECT', 'INSERT', 'UPDATE', 'DELETE');
Response
If data breached: Notify users + compliance
Patch vulnerable endpoint immediately
Add input validation/parameterized queries
Enable WAF rules
Run security audit on all endpoints
On-Call Escalation Matrix
Severity	Response Time	Who	Actions
Critical (P1)	< 15 min	Dev Lead + Senior Dev	Immediate investigation + rollback if needed
High (P2)	< 1 hour	Dev Lead	Fix or workaround
Medium (P3)	< 4 hours	Dev Team	Plan fix in next sprint
Low (P4)	Next sprint	Dev Team	Log for improvement
Post-Incident Review Template
Markdown

## Incident Report

**Date:** [Date]
**Duration:** [Start - End]
**Severity:** P1/P2/P3/P4
**Services Affected:** [Service names]

### Timeline
- 10:30 - Alert triggered: [Alert name]
- 10:35 - Identified: [Root cause]
- 10:45 - Fixed: [What was done]
- 10:50 - Verified: [How confirmed it's fixed]

### Root Cause Analysis
[Why did this happen?]

### Resolution
[What was done to fix]

### Permanent Fix
[What will prevent this in future]

### Action Items
- [ ] Code change (if needed)
- [ ] Monitoring improvement
- [ ] Documentation update
- [ ] Team training (if skill gap)

### Lessons Learned
[What we learned]
text


**Checklist:**
- [ ] All 6 runbooks created
- [ ] Team knows escalation path
- [ ] Monitoring set up for early detection
- [ ] On-call rotation defined
- [ ] Documentation in Wiki/Confluence

**Git Commit:**
```bash
git add Operations/IncidentRunbooks.md Operations/OnCallGuide.md
git commit -m "Week8-Day3-Evening: Incident response runbooks and procedures"
git push origin main
THURSDAY, NOV 19
MORNING (8-10 AM): Error Handling & Resilience Patterns
Video Search:

text

YouTube Search: "Resilience patterns - Polly, Circuit Breaker, Retry"

Duration: 45 minutes

Topics:
- Polly library deep dive
- Fallback strategies
- Bulkhead pattern
- Timeout handling
Coding Task: Enterprise Error Handling

Create file: Infrastructure/ResilienceService.cs

csharp

using Polly;
using Polly.CircuitBreaker;
using Polly.Bulkhead;
using Polly.Retry;
using Polly.Timeout;

public interface IResilienceService
{
    IAsyncPolicy<HttpResponseMessage> GetHttpRetryPolicy();
    IAsyncPolicy<T> GetDatabaseRetryPolicy<T>();
    IAsyncPolicy CreateBulkheadPolicy(int maxParallelization);
}

public class PollyResilienceService : IResilienceService
{
    private readonly ILogger<PollyResilienceService> _logger;
    
    public PollyResilienceService(ILogger<PollyResilienceService> logger)
    {
        _logger = logger;
    }
    
    /// <summary>
    /// HTTP Resilience Policy for external API calls
    /// Handles: Transient failures, timeouts, degraded service
    /// </summary>
    public IAsyncPolicy<HttpResponseMessage> GetHttpRetryPolicy()
    {
        // Retry policy: Exponential backoff
        var retryPolicy = Policy
            .Handle<HttpRequestException>()
            .Or<TimeoutRejectedException>()
            .OrResult<HttpResponseMessage>(
                r => !r.IsSuccessStatusCode && 
                r.StatusCode != System.Net.HttpStatusCode.NotFound
            )
            .WaitAndRetryAsync(
                retryCount: 3,
                sleepDurationProvider: attempt =>
                    TimeSpan.FromSeconds(Math.Pow(2, attempt)), // 2s, 4s, 8s
                onRetry: (outcome, timespan, retryCount, context) =>
                {
                    _logger.LogWarning(
                        $"Retry #{retryCount} after {timespan.TotalSeconds}s. " +
                        $"Reason: {outcome.Exception?.Message ?? "HTTP error"}"
                    );
                }
            );
        
        // Circuit Breaker: Stop calling if service is down
        var circuitBreakerPolicy = Policy
            .Handle<HttpRequestException>()
            .OrResult<HttpResponseMessage>(
                r => !r.IsSuccessStatusCode
            )
            .CircuitBreakerAsync(
                handledEventsAllowedBeforeBreaking: 5,
                durationOfBreak: TimeSpan.FromSeconds(30),
                onBreak: (outcome, duration) =>
                {
                    _logger.LogError(
                        $"Circuit breaker OPEN for {duration.TotalSeconds}s. " +
                        $"Service appears to be down."
                    );
                },
                onReset: () =>
                {
                    _logger.LogInformation("Circuit breaker RESET. Service recovered.");
                }
            );
        
        // Timeout: Don't wait forever
        var timeoutPolicy = Policy.TimeoutAsync<HttpResponseMessage>(
            TimeSpan.FromSeconds(10),
            TimeoutStrategy.Optimistic
        );
        
        // Fallback: If all fails, return cached response
        var fallbackPolicy = Policy<HttpResponseMessage>
            .Handle<Exception>()
            .OrResult(r => !r.IsSuccessStatusCode)
            .FallbackAsync(
                async (context) =>
                {
                    _logger.LogWarning("Using fallback response (cached data)");
                    // Return cached response if available
                    return new HttpResponseMessage
                    {
                        StatusCode = System.Net.HttpStatusCode.OK,
                        Content = new StringContent("Fallback cached response")
                    };
                }
            );
        
        // Combine all policies (order matters!)
        return Policy.WrapAsync(
            retryPolicy,
            circuitBreakerPolicy,
            timeoutPolicy,
            fallbackPolicy
        );
    }
    
    /// <summary>
    /// Database Resilience Policy
    /// Handles: Connection timeouts, deadlocks, transient errors
    /// </summary>
    public IAsyncPolicy<T> GetDatabaseRetryPolicy<T>()
    {
        return Policy<T>
            .Handle<TimeoutException>()
            .Or<InvalidOperationException>(
                ex => ex.Message.Contains("timeout")
            )
            .OrResult(r => r == null) // Handle null as failure
            .WaitAndRetryAsync(
                retryCount: 4,
                sleepDurationProvider: attempt =>
                    TimeSpan.FromMilliseconds(Math.Pow(2, attempt) * 100),
                // 200ms, 400ms, 800ms, 1600ms
                onRetry: (outcome, timespan, retryCount, context) =>
                {
                    _logger.LogWarning(
                        $"DB Retry #{retryCount} after {timespan.TotalMilliseconds}ms"
                    );
                }
            );
    }
    
    /// <summary>
    /// Bulkhead Pattern: Isolate resource pools
    /// Prevents: One bad endpoint crashing everything
    /// </summary>
    public IAsyncPolicy CreateBulkheadPolicy(int maxParallelization)
    {
        return Policy.BulkheadAsync(
            maxParallelizationCount: maxParallelization,
            maxQueuingActions: maxParallelization * 2,
            onBulkheadRejectedAsync: (context) =>
            {
                _logger.LogWarning(
                    "Bulkhead rejected - too many concurrent requests"
                );
                return Task.CompletedTask;
            }
        );
    }
}

// Usage in dependency injection
public static void AddResiliencePolicies(
    this IServiceCollection services)
{
    services.AddScoped<IResilienceService, PollyResilienceService>();
    
    // Register named HTTP clients with policies
    var resilienceProvider = services.BuildServiceProvider()
        .GetRequiredService<IResilienceService>();
    
    services.AddHttpClient("PaymentGateway")
        .AddPolicyHandler(resilienceProvider.GetHttpRetryPolicy());
    
    services.AddHttpClient("ThirdPartyAPI")
        .AddPolicyHandler(resilienceProvider.GetHttpRetryPolicy());
}

// Usage in service
public class PaymentService
{
    private readonly HttpClient _httpClient;
    private readonly IResilienceService _resilience;
    
    [HttpPost("process-payment")]
    public async Task<PaymentResult> ProcessPayment(PaymentRequest request)
    {
        var policy = _resilience.GetHttpRetryPolicy();
        
        var response = await policy.ExecuteAsync(async () =>
        {
            return await _httpClient.PostAsJsonAsync(
                "https://payment-gateway.com/api/process",
                request
            );
        });
        
        if (!response.IsSuccessStatusCode)
            throw new PaymentFailedException("Payment failed after retries");
        
        return await response.Content.ReadAsAsync<PaymentResult>();
    }
}
Real-World Scenarios to Implement:

Markdown

# Resilience Scenarios for Your System

## Scenario 1: Third-Party Payment Gateway Down
- 🎯 User tries to buy
- ❌ Payment service is down
- ✅ Retry 3 times (2s, 4s, 8s intervals)
- ✅ Circuit breaker opens (stops hammering)
- ✅ Return user message: "Please try again in 30 seconds"
- ✅ Log incident for manual review

## Scenario 2: Database Connection Pool Exhausted
- 🎯 High traffic spike (10K users simultaneous)
- ❌ No DB connections available
- ✅ Bulkhead: Queue requests (max 200 in queue)
- ✅ Reject if queue full (return 429 status)
- ✅ Scale up DB connections

## Scenario 3: Cascading Failures
- 🎯 Order Service calls Payment Service
- ❌ Payment Service is down
- ✅ Circuit breaker opens (fails fast)
- ✅ Order Service uses fallback (queue order, process later)
- ✅ System stays partially operational

## Scenario 4: Slow External API
- 🎯 Get user details from external service
- ❌ API responding in 30 seconds
- ✅ Timeout after 10 seconds
- ✅ Use cached user data instead
- ✅ Log to investigate why API is slow
Checklist:

 Polly policies configured
 Retry logic with exponential backoff
 Circuit breaker implemented
 Bulkhead pattern for resource isolation
 Fallback strategies defined
 Tested all failure scenarios
Git Commit:

Bash

git add Infrastructure/ResilienceService.cs Infrastructure/ResiliencePolicies.cs
git commit -m "Week8-Day4-Morning: Enterprise resilience patterns with Polly"
git push origin main
EVENING (9-10 PM): Security Hardening & OWASP
Coding Task: Production Security Checklist

Create file: Security/SecurityHardeningGuide.md

Markdown

# OWASP Top 10 + .NET Security Hardening

## 1️⃣ Injection (SQL, Command, LDAP)

### ❌ VULNERABLE:
```csharp
// SQL Injection vulnerability
string query = $"SELECT * FROM Users WHERE Username = '{username}'";
// Input: admin' OR '1'='1
// Becomes: SELECT * FROM Users WHERE Username = 'admin' OR '1'='1'
// Returns ALL users!
✅ SECURE:
csharp

// Parameterized query (safe)
string query = "SELECT * FROM Users WHERE Username = @username";
using (SqlCommand cmd = new SqlCommand(query, connection))
{
    cmd.Parameters.AddWithValue("@username", username);
    // SQL server treats input as data, not code
}

// Or with Entity Framework (safer)
var user = _db.Users.FirstOrDefault(u => u.Username == username);
// EF Core uses parameterized queries by default
Action Items:
 Audit all database queries in codebase
 Replace string concatenation with parameterized queries
 Use Entity Framework instead of raw SQL where possible
 Add SQL Injection tests
2️⃣ Broken Authentication
❌ VULNERABLE:
csharp

// Weak password requirements
public bool IsValidPassword(string password)
{
    return password.Length > 4; // Too weak!
}

// No rate limiting on login
[HttpPost("login")]
public IActionResult Login(LoginRequest request)
{
    // Anyone can brute force unlimited attempts
}

// Tokens never expire
var token = GenerateJwt(user, expiresIn: 100 * 365); // 100 years!

// Session fixation vulnerability
✅ SECURE:
csharp

// Strong password policy
public (bool isValid, string reason) ValidatePassword(string password)
{
    var requirements = new List<string>();
    
    if (password.Length < 12)
        requirements.Add("Min 12 characters");
    
    if (!Regex.IsMatch(password, @"[A-Z]"))
        requirements.Add("Uppercase letter required");
    
    if (!Regex.IsMatch(password, @"[a-z]"))
        requirements.Add("Lowercase letter required");
    
    if (!Regex.IsMatch(password, @"[\d]"))
        requirements.Add("Digit required");
    
    if (!Regex.IsMatch(password, @"[!@#$%^&*]"))
        requirements.Add("Special character required");
    
    return (requirements.Count == 0, 
            string.Join(", ", requirements));
}

// Rate limiting on login attempts
[HttpPost("login")]
[RateLimiter("LoginLimiter")] // Max 5 attempts/minute per IP
public async Task<IActionResult> Login(LoginRequest request)
{
    // Implementation
}

// Short-lived tokens
var accessToken = GenerateJwt(user, expiresIn: TimeSpan.FromMinutes(15));
var refreshToken = GenerateRefreshToken(user, expiresIn: TimeSpan.FromDays(7));

// 2FA enforcement
[HttpPost("login")]
public async Task<IActionResult> LoginWithMFA(LoginRequest request)
{
    // Generate 2FA code
    // User must provide TOTP/SMS code
    // Only then issue JWT token
}
Action Items:
 Implement password policy validation
 Add rate limiting to all auth endpoints
 Set token expiration (15-30 min access, 7 day refresh)
 Enforce 2FA for admin/sensitive operations
 Add login audit logging
 Test account lockout after 5 failed attempts
3️⃣ Sensitive Data Exposure
❌ VULNERABLE:
csharp

// Storing passwords in plain text
public class User
{
    public string Password { get; set; } // NEVER DO THIS!
}

// Logging sensitive data
_logger.LogInformation($"User login: {username}, {password}");

// Sending passwords in API response
return Ok(new { password = user.Password });

// No encryption for data in transit
// Using HTTP instead of HTTPS
✅ SECURE:
csharp

// Encrypt sensitive data at rest
public class SecureDataService
{
    private readonly IConfiguration _config;
    
    public string EncryptData(string plainText)
    {
        using (var aes = Aes.Create())
        {
            aes.Key = Encoding.UTF8.GetBytes(_config["Encryption:Key"]);
            aes.IV = Encoding.UTF8.GetBytes(_config["Encryption:IV"]);
            
            using (var encryptor = aes.CreateEncryptor(aes.Key, aes.IV))
            using (var ms = new MemoryStream())
            {
                using (var cs = new CryptoStream(ms, encryptor, CryptoStreamMode.Write))
                {
                    using (var sw = new StreamWriter(cs))
                    {
                        sw.Write(plainText);
                    }
                    return Convert.ToBase64String(ms.ToArray());
                }
            }
        }
    }
}

// Never log sensitive data
_logger.LogInformation("User {UserId} logged in", userId);
// Don't include: password, SSN, credit card, tokens

// Use DTO for API responses (never expose passwords)
public class UserDto
{
    public int UserId { get; set; }
    public string Username { get; set; }
    public string Email { get; set; }
    // NO Password field!
}

// Force HTTPS in startup
app.UseHsts();
app.UseHttpsRedirection();
Action Items:
 Identify all places sensitive data is logged (audit)
 Remove from logs
 Use DTOs in API responses
 Enforce HTTPS (HSTS headers)
 Encrypt data in Azure Key Vault
 Use Azure SQL Transparent Data Encryption (TDE)
4️⃣ XML External Entity (XXE)
❌ VULNERABLE:
csharp

// Parsing untrusted XML without safeguards
XmlDocument doc = new XmlDocument();
doc.LoadXml(userProvidedXml); // User can provide malicious XML
✅ SECURE:
csharp

XmlDocument doc = new XmlDocument();

// Disable dangerous features
doc.XmlResolver = null; // Prevent XXE
doc.DtdProcessing = DtdProcessing.Prohibit;

// Then load safely
doc.LoadXml(userProvidedXml);
Action Items:
 Check if you parse any XML
 Disable DTD processing
 Use JSON over XML (safer)
5️⃣ Broken Access Control
❌ VULNERABLE:
csharp

// No authorization check
[HttpDelete("users/{userId}")]
public async Task<IActionResult> DeleteUser(int userId)
{
    // Any authenticated user can delete ANY user!
    await _userService.DeleteAsync(userId);
    return Ok();
}

// Client-side security only
// if (user.IsAdmin) { show delete button }
// But anyone can call API directly!
✅ SECURE:
csharp

[HttpDelete("users/{userId}")]
[Authorize(Roles = "Admin")] // Server-side check
[RequiresClaim("permission", "delete_users")] // Fine-grained
public async Task<IActionResult> DeleteUser(int userId)
{
    // Additional check: Can user delete this specific user?
    var currentUserId = User.FindFirst("UserId").Value;
    
    if (currentUserId != userId && !User.IsInRole("Admin"))
        return Forbid("Cannot delete other users");
    
    await _userService.DeleteAsync(userId);
    
    // Audit log
    await _auditService.LogAsync(
        userId: currentUserId,
        action: "DELETE_USER",
        targetId: userId
    );
    
    return Ok();
}
Role-Based Access Control (RBAC):

csharp

public enum UserRole
{
    Admin,      // Full access
    Moderator,  // Manage content
    User        // Can only modify own content
}

[HttpPut("tweets/{tweetId}")]
public async Task<IActionResult> UpdateTweet(long tweetId, UpdateTweetRequest request)
{
    var tweet = await _db.Tweets.FindAsync(tweetId);
    var currentUserId = long.Parse(User.FindFirst("UserId").Value);
    
    // Check ownership first
    if (tweet.UserId != currentUserId)
    {
        // Only admin can edit others' tweets
        if (!User.IsInRole("Admin"))
            return Forbid();
    }
    
    // Update
    tweet.Content = request.Content;
    await _db.SaveChangesAsync();
    
    return Ok();
}
Action Items:
 Audit ALL endpoints for authorization checks
 Implement role-based access
 Add per-resource authorization (user owns resource)
 Document access control matrix
 Test unauthorized access scenarios
6️⃣ Security Misconfiguration
❌ VULNERABLE:
text

- Debug mode ON in production
- Detailed error messages showing stack traces
- Default credentials unchanged
- Unnecessary services running
- CORS allows all origins
- No security headers
- API keys in code/config files
✅ SECURE:
appsettings.json:

JSON

{
  "Logging": {
    "LogLevel": {
      "Default": "Warning", // Info/Debug in dev only
      "Microsoft": "Warning"
    }
  },
  "AllowedHosts": "techtweetapp.com,api.techtweetapp.com",
  "Cors": {
    "AllowedOrigins": ["https://techtweetapp.com"],
    "AllowedMethods": ["GET", "POST", "PUT", "DELETE"],
    "AllowedHeaders": ["Authorization", "Content-Type"]
  }
}
Startup configuration:

csharp

// Development vs Production
if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseSwagger(); // Swagger only in dev
}
else
{
    // Production: Generic error pages
    app.UseExceptionHandler("/error");
    app.UseHsts();
    app.UseHttpsRedirection();
}

// Security headers
app.Use(async (context, next) =>
{
    context.Response.Headers.Add(
        "X-Content-Type-Options", "nosniff"
    );
    context.Response.Headers.Add(
        "X-Frame-Options", "DENY"
    );
    context.Response.Headers.Add(
        "X-XSS-Protection", "1; mode=block"
    );
    context.Response.Headers.Add(
        "Strict-Transport-Security", 
        "max-age=31536000; includeSubDomains"
    );
    
    await next();
});

// CORS properly
app.UseCors(builder => builder
    .WithOrigins("https://techtweetapp.com")
    .AllowAnyMethod()
    .AllowAnyHeader()
    .AllowCredentials());
Action Items:
 Audit appsettings for hardcoded secrets
 Move all secrets to Azure Key Vault
 Add security headers middleware
 Configure CORS restrictively
 Disable debug in production
 Setup environment-specific configs
7️⃣ Cross-Site Scripting (XSS)
❌ VULNERABLE:
HTML

<!-- User-provided content directly in HTML -->
<div>@Model.UserComment</div>
<!-- If UserComment = "<script>alert('hacked')</script>" -->
<!-- Script executes! -->
✅ SECURE:
csharp

// In .NET - Use HTML encoding
public class Tweet
{
    public string Content { get; set; }
}

// In View/API Response
public TweetDto GetTweet(long tweetId)
{
    var tweet = _db.Tweets.Find(tweetId);
    return new TweetDto
    {
        Content = HtmlEncoder.Default.Encode(tweet.Content)
        // Converts <script> to &lt;script&gt; (harmless)
    };
}

// OR in Angular (automatic)
<div>{{ tweet.content }}</div>
<!-- Angular auto-escapes by default -->

// Only use innerHTML when NECESSARY + sanitize
import { DomSanitizer } from '@angular/platform-browser';

constructor(private sanitizer: DomSanitizer) {}

getSafeHtml(html: string) {
    return this.sanitizer.sanitize(SecurityContext.HTML, html);
}
Action Items:
 Audit all places where user input is displayed
 Add HTML encoding
 Never use innerHTML directly
 Validate input server-side (not just client)
8️⃣ Insecure Deserialization
❌ VULNERABLE:
csharp

// Deserializing untrusted data
string json = GetUserProvidedJson();
var obj = JsonConvert.DeserializeObject(json);
// Attacker can inject malicious objects
✅ SECURE:
csharp

// Always deserialize to SPECIFIC type
string json = GetUserProvidedJson();

try
{
    var tweetRequest = JsonConvert.DeserializeObject<CreateTweetRequest>(json);
    // Type is known, safe
}
catch (JsonSerializationException ex)
{
    _logger.LogWarning("Invalid JSON received: {Message}", ex.Message);
    return BadRequest("Invalid request format");
}

// Use System.Text.Json (newer, faster)
var options = new JsonSerializerOptions
{
    PropertyNameCaseInsensitive = true,
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull
};

var tweet = JsonSerializer.Deserialize<CreateTweetRequest>(json, options);
Action Items:
 Always deserialize to specific types
 Add validation after deserialization
 Use JsonSchemaValidator for complex objects
 Add try-catch for JSON parsing
9️⃣ Using Components with Known Vulnerabilities
❌ VULNERABLE:
text

NuGet Package: OldLibrary 1.0 (released 2018, has known SQL injection)
Your project references it (update available: v3.2 with fix)
You don't update because "it's working fine"
✅ SECURE:
text

Practice:
1. Weekly: Run "dotnet outdated" or use OWASP Dependency-Check
2. Monthly: Update NuGet packages
3. Quarterly: Security audit

Command:
dotnet list package --outdated
dotnet outdated --include-transitive

Or use:
GitHub Dependabot (automatic PRs for updates)
WhiteSource Bolt (free VS Code extension)
Action Items:
 Audit ALL NuGet packages
 Remove unused packages
 Update to latest stable versions
 Enable Dependabot on GitHub
 Add dependency scanning to CI/CD
🔟 Insufficient Logging & Monitoring
❌ VULNERABLE:
csharp

// Silent failures
public async Task<bool> ProcessPayment(Order order)
{
    try
    {
        var result = await _paymentGateway.Process(order);
        return result; // No logging!
    }
    catch (Exception ex)
    {
        // Exception swallowed, no alert!
        return false;
    }
}
✅ SECURE:
csharp

public async Task<bool> ProcessPayment(Order order)
{
    var operationId = Guid.NewGuid(); // Trace all related logs
    
    _logger.LogInformation(
        "Payment processing started. OrderId: {OrderId}, OperationId: {OperationId}",
        order.OrderId, operationId
    );
    
    try
    {
        var stopwatch = Stopwatch.StartNew();
        var result = await _paymentGateway.Process(order);
        stopwatch.Stop();
        
        if (result)
        {
            _logger.LogInformation(
                "Payment successful. OrderId: {OrderId}, Duration: {Duration}ms",
                order.OrderId, stopwatch.ElapsedMilliseconds
            );
            
            // Track metric: successful payments
            _telemetryClient.TrackEvent("PaymentSuccessful", 
                new Dictionary<string, string> 
                { 
                    { "OrderId", order.OrderId.ToString() },
                    { "Duration", stopwatch.ElapsedMilliseconds.ToString() }
                }
            );
        }
        else
        {
            _logger.LogWarning(
                "Payment declined. OrderId: {OrderId}",
                order.OrderId
            );
        }
        
        return result;
    }
    catch (TimeoutException ex)
    {
        _logger.LogError(
            "Payment timeout. OrderId: {OrderId}, Exception: {Exception}",
            order.OrderId, ex
        );
        // Alert team (SMS, email)
        await _alertService.SendAlert("Payment service timeout");
        throw;
    }
    catch (Exception ex)
    {
        _logger.LogError(
            ex, 
            "Payment processing failed. OrderId: {OrderId}, OperationId: {OperationId}",
            order.OrderId, operationId
        );
        // Send to error tracking (Sentry, AppInsights)
        throw;
    }
}
Action Items:
 Implement structured logging (Serilog)
 Add correlation IDs to all requests
 Setup Application Insights
 Create alerting rules for critical errors
 Add health check endpoints
Summary So Far
You have a solid foundation but need:

Depth in fundamentals (SOLID, patterns, SQL optimization)
Breadth in enterprise patterns (microservices, distributed systems)
Practical portfolio (projects > certificates)
Security mindset (non-negotiable for architect)
Azure mastery (cloud is mandatory now)
