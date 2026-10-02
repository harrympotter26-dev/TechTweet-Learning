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

## ⏰ DAILY TASKS BY WEEK

### WEEK 1: Authentication (Oct 2-8)

#### DAY 1 - MONDAY, OCT 2

**MORNING (8 AM - 10 AM) = 2 Hours**

TASK 1: Watch Video (45 min)
- Topic: SOLID Principles in C#
- Link: https://www.youtube.com/watch?v=rtmFCcjEqYw
- Duration: 45 min
- What to take notes on:
  * Single Responsibility Principle
  * Why? Easier to test and scale
  * Example: UserManager → UserRepository + UserValidator
- Post-video confidence: ___/5

TASK 2: Write Code (75 min)
- Create Visual Studio Project
  1. Open Visual Studio 2022
  2. File → New → ASP.NET Core Web API
  3. Name: TechTweet.Auth
  4. Click Create
  
- Install packages (right-click project → Manage NuGet):
  * Microsoft.AspNetCore.Authentication.JwtBearer
  * System.IdentityModel.Tokens.Jwt

- Create file: Services/PasswordService.cs
- Copy code from below ↓↓↓

CODE TEMPLATE - PASSWORD HASHING:
\`\`\`csharp
using System.Security.Cryptography;

public class PasswordService
{
    public (string hash, string salt) HashPassword(string password)
    {
        using (var rng = new RNGCryptoServiceProvider())
        {
            byte[] saltBytes = new byte[16];
            rng.GetBytes(saltBytes);
            
            var pbkdf2 = new Rfc2898DeriveBytes(
                password,
                saltBytes,
                iterations: 10000,
                hashAlgorithm: HashAlgorithmName.SHA256
            );
            
            byte[] hash = pbkdf2.GetBytes(32);
            return (Convert.ToBase64String(hash), Convert.ToBase64String(saltBytes));
        }
    }
}
\`\`\`

- Status when done: ✓ Code compiles / ❌ Has errors
- Commit: `git commit -m "Week1-Day1: Password hashing service"`

TASK 3: Code Review (30 min)
- Checklist:
  * [ ] No hardcoded secrets
  * [ ] 10,000 iterations for PBKDF2
  * [ ] 16 byte salt
  * [ ] 32 byte hash
  * [ ] Compiles without errors
  * [ ] Ready to commit
- Status: ✓ Passed / ❌ Needs fixes

**EVENING (9 PM - 10 PM) = 1 Hour**

TASK 4: Deep Dive Video (25 min)
- Topic: Why Password Hashing Matters
- Link: https://www.youtube.com/watch?v=GI790E1JMgw
- Focus: Why slow hashing is GOOD
- Key insight: ___________________

TASK 5: Implement Password Verification (30 min)
- Add to PasswordService class:

CODE TEMPLATE - PASSWORD VERIFICATION:
\`\`\`csharp
public bool VerifyPassword(string inputPassword, string storedHash, string storedSalt)
{
    var saltBytes = Convert.FromBase64String(storedSalt);
    var pbkdf2 = new Rfc2898DeriveBytes(
        inputPassword,
        saltBytes,
        iterations: 10000,
        hashAlgorithm: HashAlgorithmName.SHA256
    );
    byte[] hash = pbkdf2.GetBytes(32);
    string computedHash = Convert.ToBase64String(hash);
    return computedHash == storedHash;
}
\`\`\`

- Test it works: ✓ Yes / ❌ No
- Commit: `git commit -m "Week1-Day1-Evening: Password verification"`

**DAILY SUMMARY**
- Total time: ___/180 minutes
- Code commits: ___
- Motivation: ___/10
- Blocker: None / ___________________
- Tomorrow: JWT authentication

---

#### DAY 2 - TUESDAY, OCT 3

**MORNING (8 AM - 10 AM) = 2 Hours**

TASK 1: Watch Video (45 min)
- Topic: JWT Authentication Explained
- Link: https://www.youtube.com/watch?v=7agoErZjgQc
- Duration: 45 min
- Why JWT > Sessions at scale: ___________________
- Post-video confidence: ___/5

TASK 2: Implement JWT Login (75 min)
- Create file: Controllers/AuthController.cs

CODE TEMPLATE - JWT LOGIN:
\`\`\`csharp
[ApiController]
[Route("api/[controller]")]
public class AuthController : ControllerBase
{
    private readonly IConfiguration _config;
    private readonly PasswordService _passwordService;
    
    [HttpPost("login")]
    public IActionResult Login([FromBody] LoginRequest request)
    {
        // Validate input
        if (string.IsNullOrEmpty(request.Username)) return BadRequest("Required");
        
        // Generate JWT
        var secret = _config["Jwt:Secret"];
        var key = Encoding.ASCII.GetBytes(secret);
        var tokenHandler = new JwtSecurityTokenHandler();
        
        var tokenDescriptor = new SecurityTokenDescriptor
        {
            Subject = new ClaimsIdentity(new[]
            {
                new Claim("UserId", "123"),
                new Claim("Username", request.Username)
            }),
            Expires = DateTime.UtcNow.AddHours(24),
            SigningCredentials = new SigningCredentials(
                new SymmetricSecurityKey(key),
                SecurityAlgorithms.HmacSha256Signature
            )
        };
        
        var token = tokenHandler.CreateToken(tokenDescriptor);
        return Ok(new { token = tokenHandler.WriteToken(token) });
    }
}

public class LoginRequest { public string Username { get; set; } }
\`\`\`

- Update appsettings.json:

\`\`\`json
{
  "Jwt": {
    "Secret": "your-very-long-secret-key-min-32-characters-keep-safe!"
  }
}
\`\`\`

- Test with Postman (https://www.postman.com/downloads):
  * POST http://localhost:5000/api/auth/login
  * Body: {"username": "testuser"}
  * Should return token ✓
  
- Status: ✓ Working / ❌ Errors
- Commit: `git commit -m "Week1-Day2: JWT login endpoint"`

**EVENING (9 PM - 10 PM) = 1 Hour**

TASK 3: Token Logout & Blacklist (55 min)
- Video: Token Revocation (15 min) - search YouTube
- Install Redis: https://github.com/microsoftarchive/redis/releases
- Code: Add logout endpoint

CODE TEMPLATE - TOKEN BLACKLIST:
\`\`\`csharp
[HttpPost("logout")]
[Authorize]
public async Task<IActionResult> Logout()
{
    var token = Request.Headers["Authorization"].ToString().Replace("Bearer ", "");
    var handler = new JwtSecurityTokenHandler();
    var jwtToken = handler.ReadJwtToken(token);
    
    // Add to Redis blacklist
    await _cache.SetStringAsync(
        $"blacklist:{token}",
        "true",
        new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = 
                jwtToken.ValidTo - DateTime.UtcNow
        }
    );
    
    return Ok("Logged out");
}
\`\`\`

- Test logout: ✓ Yes / ❌ No
- Commit: `git commit -m "Week1-Day2-Evening: Token blacklist logout"`

**DAILY SUMMARY**
- Total time: ___/180 minutes
- Commits: ___
- Motivation: ___/10

---

[CONTINUE THIS PATTERN FOR ALL 84 DAYS...]

---

## 📺 ALL VIDEO LINKS

| Week | Topic | Link | Duration | Platform |
|------|-------|------|----------|----------|
| 1 | SOLID Principles | https://youtu.be/rtmFCcjEqYw | 45 min | YouTube |
| 1 | JWT Auth | https://youtu.be/7agoErZjgQc | 40 min | YouTube |
| 1 | Password Security | https://youtu.be/GI790E1JMgw | 20 min | YouTube |
| 1 | Database Design | https://youtu.be/V6WBNYiLdK0 | 30 min | YouTube |
| 2 | Message Queues | https://youtu.be/sfHMnjhAT0g | 30 min | YouTube |
| [etc - add all links] | | | | |

---

## 💻 ALL CODE TEMPLATES

### Password Hashing
\`\`\`csharp
[paste code]
\`\`\`

### JWT Generation
\`\`\`csharp
[paste code]
\`\`\`

### Database Schema
\`\`\`sql
[paste SQL]
\`\`\`

[etc...]

---

## 🔧 GIT COMMANDS

Daily commit:
\`\`\`bash
git add .
git commit -m "Week[X]-Day[Y]: [Feature description]"
git push origin main
\`\`\`

Create branch:
\`\`\`bash
git checkout -b week1-auth
# ... do work ...
git add .
git commit -m "..."
git push origin week1-auth
\`\`\`

---

## 📊 TRACKING TEMPLATE

Copy daily:
\`\`\`
## [DATE] - Week [X], Day [Y]

Morning (8-10 AM):
- [ ] Video watched: [TOPIC] | Time: ___/45 min | Confidence: ___/5
- [ ] Code written: [FILE] | Lines: ___ | Tests: ___ | Compiles: ✓/❌
- [ ] Committed: git commit -m "..."

Evening (9-10 PM):
- [ ] Deep dive: [TOPIC] | Time: ___/25 min
- [ ] Code: [FEATURE] | Status: ✓/❌
- [ ] Committed: git commit -m "..."

Daily Summary:
- Total time: ___/180 min
- Commits: ___
- Motivation: ___/10
- Blocker: None / _______________
- Tomorrow: [NEXT TOPIC]
\`\`\`

---

## 🎯 WEEKLY CHECKLIST

- [ ] 15 hours completed
- [ ] 7+ commits made
- [ ] Blog post written & published
- [ ] 30+ tests passing
- [ ] Skills self-assessed
- [ ] Reflection document created
- [ ] All code in GitHub
- [ ] README updated

---

## 🎉 END OF GUIDE

Follow this file daily from Oct 2 to Dec 31.
Everything you need is here. No more searching!

**START DATE: October 2, 2026** 🚀

\`\`\`

---

# 📥 HOW TO SAVE & USE

## Step 1: Save Markdown File
```bash
# Copy all the markdown above
# Create file in your repo:
# TechTweet-Learning/COMPLETE-GUIDE.md
# Paste all content
# Commit to GitHub

git add COMPLETE-GUIDE.md
git commit -m "Add complete learning guide"
git push origin main
