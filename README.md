<h1>Authentication and Security</h1>

=>=> *Authentication is the process of verifying a user's identity*

## Authentication in Web Development

Authentication in web development refers to the process of verifying the identity of a user, device, or system trying to access a web application or service. It is a fundamental aspect of security that ensures that only authorized users are granted access to specific resources or functionalities within the application. Authentication prevents unauthorized access and protects sensitive data from being accessed by malicious actors.

In the context of web development, authentication typically involves the following steps:

1. **User Identification**: Users provide some form of identification, usually in the form of a username, email address, or unique identifier.

2. **Credentials**: Users provide their credentials, such as a password, passphrase, or other security token.

3. **Verification**: The application verifies the provided credentials against the stored credentials for the given user. This is often done by hashing and comparing the provided password against the stored hash.

4. **Access Control**: If the provided credentials are valid, the user is granted access to the requested resources or functionalities. Access control mechanisms determine what specific resources a user is allowed to access.

Authentication can be implemented in various ways, and some common methods include:

- **Username and Password**: The user provides a username and a password that are verified against stored credentials. This method is simple but can be vulnerable to attacks such as password guessing and phishing.

- **Two-Factor Authentication (2FA)**: In addition to a password, the user must provide a second piece of information, such as a one-time code sent to their mobile device, to enhance security.

- **Multi-Factor Authentication (MFA)**: Similar to 2FA, but with more than two factors involved, such as a fingerprint scan, a smart card, or biometric data.

- **Token-Based Authentication**: Users are provided with tokens (e.g., JSON Web Tokens or OAuth tokens) upon successful login. These tokens are then used for subsequent requests, eliminating the need to send the actual credentials with every request.

- **Single Sign-On (SSO)**: Users authenticate once and gain access to multiple related systems or services without the need to re-enter credentials for each one.

- **OAuth**: A protocol that allows third-party applications to access user data on a resource server without exposing the user's credentials to the third party.

- **OpenID Connect**: An extension of OAuth that provides an identity layer, allowing third-party applications to verify the identity of users based on authentication performed by an authorization server.

- **Biometric Authentication**: Using unique biological characteristics, such as fingerprints, facial recognition, or iris scans, to verify the user's identity.

Proper authentication is a critical aspect of web development, as inadequate or flawed authentication mechanisms can lead to security vulnerabilities, unauthorized access, and data breaches. Developers must choose appropriate authentication methods based on the sensitivity of the data and the application's security requirements.

## Levels of Authentication

## Levels of Authentication in Web Development

1. **Level 1: Username and Password (Single-Factor Authentication)**

   This is the most basic form of authentication. Users provide a username and a password to access the system. While simple, it's vulnerable to attacks like password guessing and phishing. It's often used for low-security applications.

2. **Level 2: Two-Factor Authentication (2FA)**

   In addition to a password, users need to provide a second piece of information, typically a one-time code sent to their registered device (like a mobile phone). This adds an extra layer of security, as even if someone knows the password, they would need the additional code to access the account.

3. **Level 3: Multi-Factor Authentication (MFA)**

   Similar to 2FA, MFA involves multiple factors for authentication. These factors can include something the user knows (password), something the user has (a physical token or mobile device), and something the user is (biometric data like fingerprints or facial recognition). MFA significantly enhances security by requiring multiple forms of verification.

4. **Level 4: Token-Based Authentication**

   Instead of sending credentials with each request, users are issued tokens upon successful login. These tokens, often in the form of JSON Web Tokens (JWTs) or OAuth tokens, are used for subsequent requests. Tokens are time-limited and can be more secure than sending passwords with every request.

5. **Level 5: Single Sign-On (SSO)**

   SSO enables users to log in once and gain access to multiple related systems or services without re-entering credentials for each one. This is especially useful for organizations with multiple applications, as it reduces the number of passwords users need to manage and simplifies the login process.

6. **Level 6: OAuth and OpenID Connect**

   OAuth is a protocol that allows third-party applications to access user data on a resource server without exposing the user's credentials. OpenID Connect extends OAuth and provides an identity layer, allowing third-party applications to verify the user's identity based on authentication performed by an authorization server.

7. **Level 7: Biometric Authentication**

   Biometric authentication involves using unique physical traits, such as fingerprints, facial features, or iris scans, to verify a user's identity. This method offers a high level of security and convenience, as these traits are difficult to replicate.

Each of these levels offers varying degrees of security and usability, and the appropriate choice depends on factors such as the sensitivity of the data being accessed, the user experience, and the potential security threats the application might face. More secure applications often implement higher levels of authentication to protect against unauthorized access and data breaches.

------------------------------------------------------------------------------------

## Level 1 : Lowest Level Security / Single Factor Authentication

In this level of Authentication we basically done the authetication with the help of Email and Password.

DISADVANTAGE : 
Following are the disadvantage of Level 1 Authentication

- **Weak Security**: Single-factor authentication relies only on something the user knows—the password. If a password is weak or easily guessable, it becomes a weak point in the security chain. Many users use common passwords, reuse passwords across multiple sites, or choose passwords that are easy to guess.

- **Password Vulnerabilities**: Passwords can be compromised through various means, such as brute-force attacks, dictionary attacks, and phishing. If a user falls for a phishing scam or their password is stolen from another source (due to data breaches), their account becomes vulnerable.

- **No Defense Against Stolen Passwords**: Once an attacker gains access to a user's password, they can easily impersonate that user. This is especially concerning if the same password is used across multiple accounts.

- **Limited Protection Against Insider Threats**: Single-factor authentication provides little defense against insider threats—situations where authorized users with malicious intentions exploit their access to an organization's systems.

- **Inconvenience for Users**: Users often face frustration due to password complexity requirements and the need to remember multiple passwords for different accounts. This might lead them to use simpler passwords or write down their passwords, which compromises security.

- **Lack of Accountability**: In environments where multiple users share the same username and password, it becomes challenging to track who performed certain actions. This can hinder accountability in case of security incidents or unauthorized actions.

- **Limited Compliance with Security Standards**: Many industry regulations and standards require stronger authentication measures to protect sensitive data. Single-factor authentication might not meet these requirements.

- **No Protection Against Keyloggers**: Keyloggers—malicious software that records keystrokes—can capture usernames and passwords as they are typed, allowing attackers to gain unauthorized access to accounts.

- **Difficulty in Detecting Unauthorized Access**: Without additional layers of authentication, it's harder to detect if an unauthorized person gains access to an account because there's no second factor to challenge their identity.

**The whole code at this level:**

```
//jshint esversion:6
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
 
const app = express();
 
app.set("view engine","ejs");
mongoose.set("strictQuery",false);
 
app.use(bodyParser.urlencoded({extended: true}));
app.use(express.static("public"));
 
mongoose.connect("mongodb://127.0.0.1:27017/userDB",{useNewUrlParser:true});
 
const userSchema = {
    Email : String,
    Password : String
};
 
const User = mongoose.model("User", userSchema);
 
app.get("/",function(req, res){
    res.render("home");
});
 
app.get("/login",function(req, res){
    res.render("login");
});
 
app.get("/register",function(req, res){
    res.render("register");
});
 
app.post("/register", async(req, res) => {
    try{
        const newUser = new User({
            Email : req.body.username,
            Password : req.body.password
        });
 
    await newUser.save();
    console.log("Successfully inserted !");
    res.render("secrets");
    }catch(err){
        console.log(err);
             }
    });
 
app.post("/login", async(req, res)=>{
    try{
        const username = req.body.username;
        const password = req.body.password;
 
        const foundUser = await User.findOne({Email : username});
        if(foundUser){
            if(foundUser.Password === password ){
                res.render("secrets");
            }
            
        }
             
    }catch(err){
        res.send(err);
    }
});
 
 
 
app.listen(3000, function(){
    console.log("Server started on port 3000.")
});      
```

---------------------------------------------------------------------------------------

## LEVEL 2:Encryption

- **`MONGOOSE ENCRYPTION:`**
Simple encryption and authentication for mongoose documents. Relies on the Node crypto module. Encryption and decryption happen transparently during save and find.

## `mongoose-encryption` Module Explanation

- **`mongoose-encryption` Module**: `mongoose-encryption` is a third-party Node.js module that can be used with the Mongoose ODM library to add encryption capabilities to fields in MongoDB documents.

- **Field-Level Encryption**: This module allows you to selectively encrypt specific fields within Mongoose schemas. Fields that contain sensitive information can be encrypted before being stored in the database.

- **Two-Step Encryption**: `mongoose-encryption` uses two layers of encryption: first, it encrypts data using the `crypto` library and a user-provided secret; second, it adds an additional layer of encryption using MongoDB's native encryption capabilities.

- **Usage Integration**: To use `mongoose-encryption`, you need to include it as a middleware in your Mongoose schema definition. You specify which fields should be encrypted, and the module handles the encryption and decryption processes.

- **Encryption Key**: You provide a secret key when setting up `mongoose-encryption`. This key is used for both encryption and decryption. It's crucial to keep this key secure, as it's central to the encryption process.

- **Initialization Vector**: The module generates an initialization vector (IV) for each encrypted field. The IV is used to ensure that the same plaintext encrypted with the same key generates different ciphertext each time.

- **Limitations**: While `mongoose-encryption` provides a convenient way to add encryption, it's important to note that it might not provide the same level of security as more advanced encryption mechanisms. It doesn't protect against certain types of attacks, such as data leakage during query processing.

- **Alternatives**: For stronger encryption needs, especially in more security-sensitive applications, you might consider using more advanced encryption libraries and practices, such as the Node.js `crypto` module or third-party encryption libraries.

- **Maintenance Status**: It's important to check the maintenance status of third-party modules before using them. As of my last knowledge update in September 2021, the `mongoose-encryption` library was maintained, but things may have changed since then.

Remember that security is a complex topic, and the choice of encryption mechanisms should be based on the specific security requirements of your application. Always research thoroughly and consider seeking advice from security experts when dealing with sensitive data and encryption.

*Here we use mongoose-encryption module.*

=>=>=>=>  During save, documents are encrypted and then signed. During find, documents are authenticated and then decrypted

- Fisrt install mongoose encryption module:
    npm i mongoose-encryption
- Then require it into `app.js`
    const encrypt = require("mongoose-encryption");
- Now make plugin for th encryption
    const secret = "ThisisourLittleSecret.";
    ```
    userSchema.plugin(encrypt, { secret: secret ,encryptedFields: ["password"] });
    ```

    *It would encrypt the data to:*
    ```
    _ct: Binary(Buffer.from("6110e0fbd043619e466ba116b266ca55e959c0b5f7c8febdaa1ea1a0641f618f871554027038aa1b123357ea3aaf583577", "hex"), 0),
  _ac: Binary(Buffer.from("61885b22d4c6d309e8938913eb1f369bcc99b667ed66371a4b2fcc383ee8a607ce5b225f6964222c225f6374225d", "hex"), 0),
    ```
    *here we are mentioning encryptedFields , because we have to encrypt only certain feild , if we do not apply this then `whole document` would be encrypted.*

    - What is `Plugin` ?
         plugin is a piece of reusable code that extends the functionality of a module or application. It allows you to add new features, behaviors, or components to your application without modifying the core codebase. Plugins enhance modularity, maintainability, and code reusability by encapsulating specific functionality in a separate module that can be easily integrated into different projects.

**Working with `dotenv`**
DotEnv is a lightweight npm package that automatically loads environment variables from a . env file into the process. env object. To use DotEnv, first install it using the command: npm i dotenv .
A dotenv is used to store “environment variables” AKA variables we need to configure our code environment. This can include information like is our project running in “production” mode or “developer” mode but most importantly, API and database keys.

=>=>=>=> "Dotenv is a zero-dependency module that loads environment variables from a .env file into process.env. Storing configuration in the environment separate from code is based on The Twelve-Factor App methodology."

*what is `enviournment variables`*
Environment variables are variables that are set outside of a program, often through a cloud provider or operating system. In Node, environment variables are a great way to securely and conveniently configure things that don't change often, like URLs, authentication keys, and passwords

- npm install dotenv
- As early as possible in your application, import and configure dotenv:

    require('dotenv').config();

*we have to create a enviourment variables*
- make a seprate file that is named as `.env`.
- Addenviournment pecific variables on new lines in the form of NAME=VALUE.
- For example:
  ```
  DB_HOST=localhost
  DB_USER=root
  DB_PASS=s1mp13
  ABAP=temp
  ```

  if we want to access the data let suppose ABAP then we can use `process.env.FEILD_NAME`
  example:process.env.ABAP


**The whole code without using env variables:**

```
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
const encrypt = require("mongoose-encryption");
 
const app = express();
 
 
app.use(express.static("public"));
app.set('view engine', 'ejs');
app.use(bodyParser.urlencoded({
  extended: true
}));
 
 
mongoose.connect("mongodb://localhost:27017/userDB", {useNewUrlParser: true});
 
const userSchema = new mongoose.Schema({
  email: String,
  password: String
});
 
const secret = "Thisisourlittlesecret.";
userSchema.plugin(encrypt, { secret: secret, encryptedFields: ["password"] });
 
const User = new mongoose.model("User", userSchema);
//
app.get("/", function(req,res){
  res.render("home");
});
 
app.get("/login", function(req,res){
  res.render("login");
});
 
app.get("/register", function(req,res){
  res.render("register");
});
 
 
app.post("/register", function(req, res){
  const newUser = new User({
    email: req.body.username,
    password: req.body.password
  });
 
  newUser.save(function(err){
    if(err){
      console.log(err);
    }else{
      res.render("secrets");
    }
  });
});
 
 
app.post("/login", function(req, res){
   const username = req.body.username;
   const password = req.body.password;
 
   User.findOne({email: username}, function(err, foundUser){
     if(err){
       console.log(err);
     }else{
       if (foundUser){
         if (foundUser.password === password){
           res.render("secrets");
         }
       }
     }
   });
});
 
 
app.listen(3000, function(){
  console.log("Server started on port 3000");
});
```

**The whole code with using env variables:**

```
require('dotenv').config();
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
const encrypt = require("mongoose-encryption");

const app = express();

app.use(express.static("public"));
app.set('view engine', 'ejs');
app.use(bodyParser.urlencoded({ extended: true }));

mongoose.connect("mongodb://localhost:27017/userDB", {
    useNewUrlParser: true,
    useUnifiedTopology: true
});

const userSchema = new mongoose.Schema({
    email: String,
    password: String
});

userSchema.plugin(encrypt, {
    secret: process.env.SECRET,
    encryptedFields: ["password"],
    requireAuthenticationCode: false
});

const User = new mongoose.model("User", userSchema);

app.get("/", (req, res) => {
    res.render("home");
})

app.get("/login", (req, res) => {
    res.render("login");
})

app.get("/register", (req, res) => {
    res.render("register");
})

app.get("/logout", (req, res) => {
    res.redirect("/");
})

app.post("/register", function (req, res) {
    const newUser = new User({
        email: req.body.username,
        password: req.body.password
    });

    newUser.save(function (err) {
        if (err) {
            console.log(err);
        } else {
            res.render("secrets");
        }
    });
});

app.post("/login", function (req, res) {

    const username = req.body.username;
    const password = req.body.password;

    User.findOne({ email: username }, function (err, foundUser) {
        if (err) {
            console.log(err);
        } else {
            if (foundUser) {
                if (foundUser.password == password) {
                    res.render("secrets");
                }
                else {
                    res.send("Your password doesn't match with the given username");
                }
            } else {
                res.send("No such user is found!");
            }
        }
    });
});

app.listen(3000, function () {
    console.log("Server is running on port 3000.");
})
```

------------------------------------------------------------------------
## LEVEL 3:Hashing

# Disadvantages of Encryption

Encryption is a vital security tool, but it comes with certain disadvantages or challenges that need to be considered. Here are some key disadvantages of encryption:

1. **Complexity**: Encryption can be complex to implement and manage, especially for organizations dealing with large volumes of data. Setting up encryption systems, managing keys, and ensuring secure key storage can be daunting tasks.

2. **Performance Impact**: Encrypting and decrypting data can introduce a performance overhead, especially on older or less powerful hardware. This can lead to slower data transfer and processing speeds.

3. **Key Management**: Securely managing encryption keys is crucial. Loss, theft, or compromise of encryption keys can result in data loss or unauthorized access. Key management can be a significant challenge for organizations.

4. **Compatibility**: Encryption standards and algorithms can evolve over time. Ensuring that encrypted data remains accessible and compatible with future systems and standards can be a concern.

5. **Data Recovery**: When data is encrypted and the encryption key is lost, recovering the data can be nearly impossible. This can be problematic if a user forgets their encryption passphrase or if a key is accidentally deleted.

6. **Legal and Compliance Issues**: Some industries and countries have strict regulations regarding encryption, which can lead to legal challenges. Certain types of encryption may be restricted or require government approval in some regions.

7. **Resource Intensive**: Strong encryption algorithms can be resource-intensive, making them less practical for low-power devices or devices with limited processing capabilities.

8. **User Experience**: Encryption can sometimes add complexity to user interfaces and workflows, which can be confusing or frustrating for users who are not familiar with encryption processes.

9. **Backdoor Concerns**: There is an ongoing debate about whether encryption should have backdoors for government or law enforcement access. While backdoors could aid investigations, they also pose significant security risks if discovered or misused.

10. **Cost**: Implementing and maintaining robust encryption systems can be expensive. It may require an investment in hardware, software, and personnel with specialized knowledge.

11. **Misconfiguration Risk**: Misconfiguring encryption settings can inadvertently weaken security or make data vulnerable. Human error in setting up encryption can lead to unintended consequences.

12. **Interoperability**: Different encryption systems and algorithms may not always be compatible with each other, making data exchange between systems a challenge.

**`Hashing`:**


Hashing in authentication refers to the process of converting a user's password or other authentication credentials into a fixed-length string of characters, which is typically a hexadecimal or base64-encoded value. This hashed value is then stored in a database. When a user attempts to log in, the system hashes the provided password and compares it to the stored hash value to verify the user's identity.

Here we use certain mathmatical function to convert the passwords in a unique hash value.

*hashing is a **one-way process**, and the original data cannot be derived from the hash value.*

Diffrence between `Encryption` and `Hashing`

| Aspect                     | Encryption                                                            | Hashing                                                                         |
| -------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **Purpose**                | Confidentiality of data.                                              | Data integrity verification, password storage.                                  |
| **Reversibility**          | Reversible - original data can be recovered with the key.             | Irreversible - original data cannot be derived from the hash.                   |
| **Key Usage**              | Requires encryption keys for both encryption and decryption.          | No keys involved; hashing is a one-way process.                                 |
| **Performance**            | Can be computationally intensive, potentially slowing down processes. | Generally faster and has consistent performance regardless of data size.        |
| **Data Size**              | Encrypted data is typically larger due to added encryption data.      | Fixed-size output (hash) regardless of input size.                              |
| **Data Recovery**          | Possible with the encryption key.                                     | Not possible as hashing is irreversible.                                        |
| **Security Focus**         | Confidentiality of data.                                              | Data integrity and password security.                                           |
| **Key Exposure Risks**     | Compromised keys can lead to data exposure.                           | Hash values are not meant to be reversible, so they don't expose data directly. |
| **Resource Intensity**     | Can be resource-intensive, especially for large data sets.            | Generally less resource-intensive.                                              |
| **Backdoors/Legal Issues** | May be subject to legal debates and backdoor discussions.             | Less susceptible to legal debates as it's primarily for data integrity.         |
| **Data Format**            | Recognizable as encrypted data.                                       | Fixed-length hash values with no specific data format.                          |

**`Hashing Algorithms`:**MD5, SHA-1, SHA-256, and bcrypt, among others, with bcrypt being particularly suitable for securely hashing passwords.

Here we use `MD5` algorithm for encryption

1. ```npm i md5```
2. ```const md5=require("md5")```
3. ```
   const newUser = new User({
        email:req.body.username,
        password:md5(req.body.password)
    });
   ```
**Output**
```
{
    "_id" : ObjectId("64f763a9ddbfcd8695f02012"),
    "email" : "user@hash.com",
    "password" : "81dc9bdb52d04dc20036dbd8313ed055",
    "__v" : NumberInt(0)
}
```

=>=>=>=> *How hacker's hack the password,even with the hashing ?*
Hacker's make a `Hash Table`of the most common passowrds and their corresponding hash value,and in this way the
password becomes vurnerable.

**Vulnerabilities in Hashing for Authentication:**

1. **Brute Force Attacks:** Attackers can attempt to guess the original password by hashing various inputs and comparing them to the stored hash until a match is found.

2. **Rainbow Table Attacks:** Attackers can use precomputed tables of hash values (rainbow tables) to quickly look up the original password corresponding to a hash.

3. **Dictionary Attacks:** Attackers use a list of common passwords and hash them, then compare the hashes to stored hashes to find a match.

4. **Salting:** Without proper salting (adding a unique value to each password before hashing), identical passwords will produce the same hash, making it easier for attackers to identify identical passwords.

5. **Collision Vulnerabilities:** In some cases, two different inputs can produce the same hash (collision). Although this is rare with strong hashing algorithms, it's a concern.

6. **Algorithm Weaknesses:** Older or weak hashing algorithms like MD5 and SHA-1 are more vulnerable to attacks due to their susceptibility to brute force and collision attacks.

7. **Hardware Acceleration:** Advancements in hardware (e.g., GPUs and ASICs) have made brute force attacks faster and more efficient.

8. **Length Extension Attacks:** Some hash functions are vulnerable to length extension attacks, where an attacker can append data to a hashed value and generate a new valid hash.

To mitigate these vulnerabilities, it's essential to use strong, slow, and salted hashing algorithms like bcrypt or Argon2, implement proper password policies, and stay updated with best practices in password security. Additionally, using multi-factor authentication (MFA) can further enhance authentication security.

### *The Whole code at Level-3*
```
//jshint esversion:6
require('dotenv').config();
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
const encrypt = require("mongoose-encryption");

const app = express();

app.use(express.static("public"));
app.set('view engine','ejs');
app.use(bodyParser.urlencoded({
    extended:true
}));

mongoose.connect("mongodb://localhost:27017/userDB");

const userSchema = new mongoose.Schema({
    email:String,
    password:String
});



userSchema.plugin(encrypt, { secret: process.env.SECRET ,encryptedFields: ["password"] });

const User = new mongoose.model("user",userSchema);


app.get("/",function(req,res){
    res.render("home");
})
app.get("/login",function(req,res){
    res.render("login");
})
app.get("/register",function(req,res){
    res.render("register");
})
app.get("/logout",function(req,res){
    res.render("home");
})


app.post("/register",function(req,res){
    const newUser = new User({
        email:req.body.username,
        password:req.body.password
    });

    newUser.save()
    .then(()=>{
        res.render("secrets");
    })
    .catch((err)=>{
        console.log(err);
    });
});

app.post("/login",function(req,res){

    const username = req.body.username;
    const password = req.body.password;

    User.findOne({email:username})
    .then(found => {
        if (found) {
            if(found.password === password){
                res.render("secrets");
            }
            else{
                res.send("Password does not match !!");
            }
        } else {
            res.send("No user were found");
        }
    })
    .catch(err => {
        console.error(err);
    });
});

app.listen(3000,function(){
    console.log("Server sarted on port 3000.");
})
```

--------------------------------------------------------------------------

## LEVEL 4:Hashing and salting

**`Salting:`**:A salt is a piece of random data added to a password before it is hashed and stored. Adding a salt to stored passwords is a security process used alongside the hashing of passwords before they are stored.

   password + salted value =>(Hash Function)=> Hash key

Popular Algorithm : `bcrypt`:
bcrypt is a password-hashing function designed by Niels Provos and David Mazières, based on the Blowfish cipher and presented at USENIX in 1999.

- What is bcrypt and how does it work?

   Bcrypt turns a simple password into fixed-length characters called a hash. Before hashing a password, bcrypt applies a salt — a unique random string that makes the hash unpredictable.

   ```npm install bcrypt```
   if this does not work or gives some error:
   ```npm install bcrpt@LOWER_VERSION```

- what is `salt Rounds`?
  
  The Salt rounds is the number of times the salting is performed on the hashvalue that we get,that means applying salting on the hash value that we previously get fro salting.

**Extra**:
---------------------------------
what is `nvm`?
Node Version Manager (NVM), as the name implies, is a tool for managing Node versions on your device. Different projects on your device may be using different versions of Node. Using only one version (the one installed by npm ) for these different projects may not give you accurate execution results.

``` nvm install LATEST_NODE_VERSION_NUMBER ```
It will update the node to the latest version.

=>>>>>>>>>  **`THE PROBLEM I FACED`**

```
i got the error:err = new ServerSelectionError();
          ^

MongooseServerSelectionError: connect ECONNREFUSED ::1:27017
    at _handleConnectionErrors (C:\Users\Bhaskar Kulshrestha\Desktop\BackEnd\Authentication\Secrets+-+Starting+Code\Secrets - Starting Code\node_modules\mongoose\lib\connection.js:805:11)
    at NativeConnection.openUri (C:\Users\Bhaskar Kulshrestha\Desktop\BackEnd\Authentication\Secrets+-+Starting+Code\Secrets - Starting Code\node_modules\mongoose\lib\connection.js:780:11)
    at runNextTicks (node:internal/process/task_queues:60:5)
    at listOnTimeout (node:internal/timers:538:9)
    at process.processTimers (node:internal/timers:512:7) {
  reason: TopologyDescription {
    type: 'Unknown',
    servers: Map(1) {
      'localhost:27017' => ServerDescription {
        address: 'localhost:27017',
        type: 'Unknown',
        hosts: [],
        passives: [],
        arbiters: [],
        tags: {},
        minWireVersion: 0,
        maxWireVersion: 0,
        roundTripTime: -1,
        lastUpdateTime: 9542110,
        lastWriteDate: 0,
        error: MongoNetworkError: connect ECONNREFUSED ::1:27017
            at connectionFailureError (C:\Users\Bhaskar Kulshrestha\Desktop\BackEnd\Authentication\Secrets+-+Starting+Code\Secrets - Starting Code\node_modules\mongodb\lib\cmap\connect.js:367:20)
            at Socket.<anonymous> (C:\Users\Bhaskar Kulshrestha\Desktop\BackEnd\Authentication\Secrets+-+Starting+Code\Secrets - Starting Code\node_modules\mongodb\lib\cmap\connect.js:290:22)
            at Object.onceWrapper (node:events:629:26)
            at Socket.emit (node:events:514:28)
            at emitErrorNT (node:internal/streams/destroy:151:8)
            at emitErrorCloseNT (node:internal/streams/destroy:116:3)
            at process.processTicksAndRejections (node:internal/process/task_queues:82:21) {
          cause: Error: connect ECONNREFUSED ::1:27017
              at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1495:16) {
            errno: -4078,
            code: 'ECONNREFUSED',
            syscall: 'connect',
            address: '::1',
            port: 27017
          },
          [Symbol(errorLabels)]: Set(1) { 'ResetPool' }
        },
        topologyVersion: null,
        setName: null,
        setVersion: null,
        electionId: null,
        logicalSessionTimeoutMinutes: null,
        primary: null,
        me: null,
        '$clusterTime': null
      }
    },
    stale: false,
    compatible: true,
    heartbeatFrequencyMS: 10000,
    localThresholdMS: 15,
    setName: null,
    maxElectionId: null,
    maxSetVersion: null,
    commonWireVersion: 0,
    logicalSessionTimeoutMinutes: null
  },
  code: undefined
}

Node.js v18.17.1 
```
To solve this problem just do the changes in:

   FROM
   ```mongoose.connect("mongodb://localhost:27017/userDB");```<BR>
   TO   
   ```mongoose.connect("mongodb://127.0.0.1:27017/userDB");```

---------------------------------

- To store the bcrypt data (For registration of the user) in the databse we use:
  ```
  bcrypt.hash(req.body.password, saltRounds, function(err, hash) {
        const newUser = new User({
            email:req.body.username,
            password:hash
        });
    
    
    
        newUser.save()
        .then(()=>{
            res.render("secrets");
        })
        .catch((err)=>{
            console.log(err);
        });
    });
  ```

- To perform Login we have to match the give password that would be converted to hash by bcrypt algorithm to the password that is present in the database.
```
bcrypt.compare(password,found.password, function(err, result) {
                if(result===true)
                {
                    res.render("secrets");
                }
                else{
                    res.send("Password does not match !!");
                }
});
```

*Whole code at LEVEL-4*
```
//jshint esversion:6
require('dotenv').config();
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
const bcrypt=require("bcrypt");
const saltRounds = 10;

const app = express();
// console.log(process.env.API_KE)
app.use(express.static("public"));
app.set('view engine','ejs');
app.use(bodyParser.urlencoded({
    extended:true
}));

mongoose.connect("mongodb://127.0.0.1:27017/userDB");

const userSchema = new mongoose.Schema({
    email:String,
    password:String
});


const User = new mongoose.model("user",userSchema);


app.get("/",function(req,res){
    res.render("home");
})
app.get("/login",function(req,res){
    res.render("login");
})
app.get("/register",function(req,res){
    res.render("register");
})
app.get("/logout",function(req,res){
    res.render("home");
})



app.post("/register",function(req,res){

    bcrypt.hash(req.body.password, saltRounds, function(err, hash) {
        const newUser = new User({
            email:req.body.username,
            password:hash
        });
    
    
    
        newUser.save()
        .then(()=>{
            res.render("secrets");
        })
        .catch((err)=>{
            console.log(err);
        });
    });

   
});

app.post("/login",function(req,res){

    const username = req.body.username;
    const password = req.body.password;

    User.findOne({email:username})
    .then(found => {
        if (found) {
            bcrypt.compare(password,found.password, function(err, result) {
                if(result===true)
                {
                    res.render("secrets");
                }
                else{
                    res.send("Password does not match !!");
                }
            });
            
        } else {
            res.send("No user were found");
        }
    })
    .catch(err => {
        console.error(err);
    });
});

app.listen(3000,function(){
    console.log("Server sarted on port 3000.");
})
```

--------------------------------------------------------------------------------

## LEVEL 5: Cookies and Sessions

![cookie and session figure](https://www.researchgate.net/publication/314289445/figure/fig1/AS:660237199618050@1534424223601/Cookie-based-User-Authentication.png) <br>
[cookie and session figure](https://www.researchgate.net/publication/314289445/figure/fig1/AS:660237199618050@1534424223601/Cookie-based-User-Authentication.png)

`COOKIES`  is a small piece of data that a web server sends to a user's web browser. The browser then stores this data and sends it back to the server with subsequent requests. USED TO:session management,personalzation,Tracking and Analytics,Authentication,Shopping Carts,Target Advertismets.

Cookies can be categorised in two mian types:
- Session Cookies: These cookies are temporary and are stored only for the duration of a user's session. Once the user closes their browser, session cookies are typically deleted.
- Persistent Cookies: Persistent cookies have an expiration date and remain on the user's device even after the browser is closed. They are used for longer-term purposes, such as remembering login credentials or user preferences

EXAMPLE:
"" 
Certainly! Think of a cookie on the internet like a small note that a website gives to your web browser. This note contains information or instructions. Your web browser keeps this note and shows it to the website whenever you visit again.

Imagine you're in a library, and you get a sticker with your name on it when you enter. As long as you have that sticker, the library staff knows who you are and what books you've borrowed. Cookies work kind of like that sticker, helping websites remember you and your preferences when you return. They make your internet experience more personalized and convenient. ""

![](Images/cookie%20making.jpg)
![](Images/Cookie%20Using.jpg)

`SESSION` refers to a temporary and interactive connection between a user's device (such as a computer or smartphone) and a remote server or service. During a session, data can be exchanged between the user and the server in real-time.

EXAMPLE:
 the session is like a shopping cart that keeps track of your selected items and other important information until you finish your shopping and check out. It ensures that your online shopping experience is smooth, and it's a fundamental concept in many online interactions, from shopping to social media to email.

*Diffrence between cookie and session*
| Aspect                | Sessions                                                                                                              | Cookies                                                                                         |
| --------------------- | --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **Storage Location**  | Server-side (typically on the server).                                                                                | Client-side (stored in the browser).                                                            |
| **Data Storage**      | Can store a larger amount of data.                                                                                    | Limited in size (usually a few kilobytes).                                                      |
| **Lifespan**          | Typically short-lived and temporary.                                                                                  | Can be short-lived or long-lived, depending on type (session or persistent).                    |
| **Usage**             | Used to store user-specific data and maintain state during a user's interaction with a web application.               | Used for storing small pieces of data, often for personalization or tracking.                   |
| **Security**          | More secure because data is stored on the server, reducing exposure to client-side attacks.                           | Less secure because data is stored on the client's device and can be tampered with or accessed. |
| **Data Transmission** | No data is transmitted to the server with each request; session ID or token is used.                                  | Sent with every HTTP request to the server, which can increase network traffic.                 |
| **Examples**          | Maintaining a user's shopping cart during an online shopping session, managing user authentication and authorization. | Storing user preferences, tracking website analytics, and maintaining user login status.        |


**`PASSPORT`:** Passport is authentication middleware for Node.js. Extremely flexible and modular, Passport can be unobtrusively dropped in to any Express-based web application. A comprehensive set of strategies support authentication using a username and password, Facebook, Twitter, and more.

- Passport is an authentication middleware for Node. js which we are going to use for `session management`.
- =>=>=> `Alternatives` of Passport:
   JSON Web Token, Auth0, Keycloak, Amazon Cognito, and OAuth2 are the most popular alternatives and competitors to Passport.

Now we will install the given packages

- passport
- passport-local
- passport-local-mongoose
- express-session
  
```
npm i passport passport-local passport-local-mongoose express-session
```

- Now We will make the `session` by paste the following code just after all the app.use methods and just above all mongoose.connect:
- DOC: [Express-Session](https://www.npmjs.com/package/express-session)   
  
  ```
  app.use(session({
    secret: "Our little secret.",
    resave: false,
    saveUninitialized: true,
  }));
  ```

`saveUninitialized`:Forces a session that is "uninitialized" to be saved to the store. A session is uninitialized when it is new but not modified. Choosing false is useful for implementing login sessions, reducing server storage usage, or complying with laws that require permission before setting a cookie. Choosing false will also help with race conditions where a client makes multiple parallel requests without a session.

Now give the following code:

- app.use(passport.initialize());  //initialize the session.
- app.use(passport.session()); //Manage our session.


`passport-local-mongoose`:Passport-Local Mongoose is a Mongoose plugin that simplifies building username and password login with Passport.

- First you need to plugin Passport-Local Mongoose into your User schema
  

```
userSchema.plugin(passportlocalmongoose);
```

`passport.serializeUser()` and `passport.deserializeUser()` :

Passport uses the serializeUser function to take the user object and convert it into a session object.Passport stores the session object on the server and sends a session cookie to the user's browser.

and deserializeUser function do just opposite.

=>=>=>  passport.serializeUser() is setting id as cookie in user’s browser and passport.deserializeUser() is getting id from the cookie, which is then used in callback to get user info or something else, based on that id or some other piece of information from the cookie.

- =>=>=> Now write the `passport-Local Configuration`:
  
```
passport.use(User.createStrategy());

passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```

=>=>=>=> Now we will REGISTER our user:

```
app.post("/register", function (req, res) {

    User.register({username:req.body.username},req.body.password,function(err,user){
        if(err){
            console.log(err);
            res.redirect("/register");
        }
        else{
            passport.authenticate("local")(req,res,function(){
                res.redirect("/secrets");
            })
        }
    })

});
```


The general idea of the above code can be given by the following code:

```
User.register({username:'username', active: false}, 'password', function(err, user) {
  if (err) { ... }

  const authenticate = User.authenticate();
  authenticate('username', 'password', function(err, result) {
    if (err) { ... }

    // Value 'result' is set to false. The user could not be authenticated since the user is not active
  });
});

```

- `authenticate()` Generates a function that is used in Passport's LocalStrategy.
- `register(user, password, cb)` Convenience method to register a new user instance with a given password. Checks if username is unique.

Now at the time the session is maintained then the user need not to login again because of the cookie, so to apply this logic we have to make the changes in the route permission of the page,
and the session is activated till the browser is open.

```
app.get("/secrets",function(req,res){
    if(req.isAuthenticated()){
        res.render("secrets");
    }
    else{
        res.redirect("/login");
    }
});
```

Now we will implement the login function with the help of PASSPORT.

**HELPING CODE:**

```
req.login(user, function(err) {
  if (err) { return next(err); }
  return res.redirect('/users/' + req.user.username);
});
```

the main code is:

```
app.post("/login", function (req, res) {

    const user=new User({
        username:req.body.username,
        password:req.body.password
    });
    req.login(user,function(err){
        if(err)
        {
            console.log(err);
        }
        else{
            passport.authenticate("local")(req,res,function(){
                res.redirect("/secrets");
            });
        }
    })

});
```

`isAuthenticated` checks that the current user is authenticated or not,it is the function of `passport`.

To deAuthenticate the user:

```
app.post('/logout', function(req, res, next){
  req.logout(function(err) {
    if (err) { return next(err); }
    res.redirect('/');
  });
});
```

=>=>=>=> **The cookie will be deleted when we restart the server. for example by the command `rs`**

### Whole code for the Authentication with `cookie` and `session`:

```
//jshint esversion:6
require('dotenv').config();
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
const session = require('express-session');
const passport = require("passport");
const passportlocalmongoose = require("passport-local-mongoose")

const app = express();
// console.log(process.env.API_KE)
app.use(express.static("public"));
app.set('view engine', 'ejs');
app.use(bodyParser.urlencoded({
    extended: true
}));

app.use(session({
    secret: "Our little secret.",
    resave: false,
    saveUninitialized: true,
}));

app.use(passport.initialize());
app.use(passport.session());

mongoose.connect("mongodb://127.0.0.1:27017/userDB");

const userSchema = new mongoose.Schema({
    email: String,
    password: String
});

userSchema.plugin(passportlocalmongoose);

const User = new mongoose.model("user", userSchema);

passport.use(User.createStrategy());

passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());


app.get("/", function (req, res) {
    res.render("home");
})
app.get("/login", function (req, res) {
    res.render("login");
})
app.get("/register", function (req, res) {
    res.render("register");
})
app.get("/logout", function (req, res) {
    res.render("home");
})
app.get("/secrets",function(req,res){
    if(req.isAuthenticated()){
        res.render("secrets");
    }
    else{
        res.redirect("/login");
    }
});
app.get("/logout",function(req,res){
    req.logout();
    res.redirect("/");
});

app.post("/register", function (req, res) {

    User.register({username:req.body.username},req.body.password,function(err,user){
        if(err){
            console.log(err);
            res.redirect("/register");
        }
        else{
            passport.authenticate("local")(req,res,function(){
                res.redirect("/secrets");
            });
        }
    })

});

app.post("/login", function (req, res) {

    const user=new User({
        username:req.body.username,
        password:req.body.password
    });
    req.login(user,function(err){
        if(err)
        {
            console.log(err);
        }
        else{
            passport.authenticate("local")(req,res,function(){
                res.redirect("/secrets");
            });
        }
    })

});

app.listen(3000, function () {
    console.log("Server sarted on port 3000.");
})
```


--------------------------------------------------------------------------------

## LEVEL 6: OAuth 2.0 and how to implement sign in with Gooogle (Third party OAuth)

OAuth 2.0, which stands for “Open Authorization”, is a standard designed to allow a website or application to access resources hosted by other web apps on behalf of a user.

Basically, OAuth 2.0 is a security standard where you give one application permission to access your data in another application. The steps to grant permission, or consent, are often referred to as authorization or even delegated authorization.

**Why OAuth ?**

1. `Granular Access Levels` : it allows organizations to define specific access policies for different users or groups rather than relying on broad, one-size-fits-all access controls.That means we can request specific things from the third party account.
2. provide `Read / Read + write` Access 
3. `Revoke` Access : any newly generated data will no longer be shared with a recipient previously granted access to the data , That means the usermay go to the third party account and DEAUTHORIZE the access that they granted.

  ### *Steps for OAuth:*
   
    1. Set up Your app,tell the third party about our application
    2. Redirect to Authenticate
    3. User LogsIn
    4. User Grants the Permission
    5. Receive Authorization Code
    6. Exchange Authorization code for the access Token


## =>=>=>=>  Implementing Authorization using Passport Google Authorization

Go to the Passport and search for the [passport-google-oauth20](https://www.passportjs.org/packages/passport-google-oauth20/) in strategy.


1. ```npm install passport-google-oauth20```
2. Now register in [Google Developer Console](https://console.cloud.google.com/apis/dashboard)
3. go to the project which is in the mid top and `create new project`.
4. Now go to OAuth consent screen,and cick external user and click create.
5. Now fill the information form and fillonly neccesary credentials
6. Now click on `add scope`
   Now search the following an add
   1. Email
   2. profile
   3. openid
7. Now complete the process and go back to dash board
8. Now go to `crdentials` and click create credentials and click `OAuth` client ID.
9. Application type:Web application.
10. provide a name
11. Now in `Authorised JavaScript origins` give the ```http://localhost:3000```.
12. Now in `Authorised redirect URIs` give ```http://localhost:3000/auth/google/secrets```(As in this project)
13. =>=>=>=>=> **Now copy the clientID and the client secrets and paste them in the .env file with the env syntax(assign in the variables in capital letters).**
14. Now go to `Passport`.
15. Now apply
    
    ```
    const GoogleStrategy = require('passport-google-oauth20').Strategy;
    ```
16. 
```
passport.use(new GoogleStrategy({
    clientID: process.env.CLIENT_ID (****  as in .env file),
    clientSecret: process.env.CLIENT_SECRET (****  as in .env file),
    callbackURL: "http://localhost:3000/auth/google/secrets",
    userProfileURL: "https://www.googleapis.com/oauth2/v3/userinfo"
},
    function (accessToken, refreshToken, profile, cb) {
        console.log(profile);
        User.findOrCreate({ googleId: profile.id }, function (err, user) {
            return cb(err, user);
        });
    }
));
```
17. Now to use `findorcreate` we would install npm package:
    ```npm install mongoose-findorcreate```

18. Now add ```const findOrCreate = require('mongoose-findorcreate');```
19. Now add the plugin
    ```
    userSchema.plugin(passportlocalmongoose);
    userSchema.plugin(findOrCreate);
    ```
20.  Now get `auth/google` with the code:
    
```
    app.get('/auth/google',
  passport.authenticate('google', { scope: ['profile'] }));

app.get('/auth/google/secrets', 
  passport.authenticate('google', { failureRedirect: '/login' }),
  function(req, res) {
    // Successful authentication, redirect secrets page.
    res.redirect('/secrets');
});
```
21. Now we change the `serializeUser` and `deserializeUser` from local strategy to global:
```
passport.serializeUser(function(user,done){
    done(null,user.id);
});
passport.deserializeUser(function(id,done){
    User.findById(id)
  .then(user => done(null, user))
  .catch(err => done(err));
});
```
22. Now add google id in the userSchema:
```
const userSchema = new mongoose.Schema({
email: String,
password: String,
googleId:String,
facebookId:String,
githubId:String
});
```
*Styling of button*
23. go to social buttons for bootstrap and downlload the file:
24. Now drag the `bootstrap-social.css` file into public/css folder.
25. Now link this css file in the head tag using link tag.
26. Now apply the class `btn-social` and `btn-google`.
```
<div class="card-body">
  <a class="btn btn-block btn-social btn-google" href="/auth/google" role="button">
    <i class="fab fa-google"></i>
    Sign Up with Google
  </a>
</div>
```

**The whole code:**

```
//jshint esversion:6
require('dotenv').config();
const express = require("express");
const bodyParser = require("body-parser");
const ejs = require("ejs");
const mongoose = require("mongoose");
const session = require('express-session');
const passport = require("passport");
const passportlocalmongoose = require("passport-local-mongoose");
const GoogleStrategy = require('passport-google-oauth20').Strategy;
const FacebookStrategy = require('passport-facebook').Strategy;
const GitHubStrategy = require('passport-github2').Strategy;

const findOrCreate = require('mongoose-findorcreate');


const app = express();
// console.log(process.env.API_KE)
app.use(express.static("public"));
app.set('view engine', 'ejs');
app.use(bodyParser.urlencoded({
  extended: true
}));

app.use(session({
  secret: "Our little secret.",
  resave: false,
  saveUninitialized: true,
}));

app.use(passport.initialize());
app.use(passport.session());

mongoose.connect("mongodb://127.0.0.1:27017/userDB");

const userSchema = new mongoose.Schema({
  email: String,
  password: String,
  secret:String,
  googleId: String,
  facebookId: String,
  githubId: String,
});

userSchema.plugin(passportlocalmongoose);
userSchema.plugin(findOrCreate);

const User = new mongoose.model("user", userSchema);

passport.use(User.createStrategy());



passport.serializeUser(function (user, done) {
  done(null, user.id);
});
passport.deserializeUser(function (id, done) {
  User.findById(id)
    .then(user => done(null, user))
    .catch(err => done(err));
});


//LOGIN USING GOOGLE
passport.use(new GoogleStrategy({
  clientID: process.env.CLIENT_ID,
  clientSecret: process.env.CLIENT_SECRET,
  callbackURL: "http://localhost:3000/auth/google/secrets",
  userProfileURL: "https://www.googleapis.com/oauth2/v3/userinfo"
},
  function (accessToken, refreshToken, profile, cb) {
    // console.log(profile);
    User.findOrCreate({ googleId: profile.id }, function (err, user) {
      return cb(err, user);
    });
  }
));


app.get("/", function (req, res) {
  res.render("home");
});

//from passport 
app.get('/auth/google',
  passport.authenticate('google', { scope: ['profile'] }));

app.get('/auth/google/secrets',
  passport.authenticate('google', { failureRedirect: '/login' }),
  function (req, res) {
    // Successful authentication, redirect secrets page.
    res.redirect('/secrets');
  });

//LOGIN THROUGH FACEBOOK   

passport.use(new FacebookStrategy({
  clientID: process.env.FACEBOOK_CLIENT_ID,
  clientSecret: process.env.FACEBOOK_SECRET,
  callbackURL: "http://localhost:3000/auth/facebook/secrets"
},
  function (accessToken, refreshToken, profile, cb) {
    User.findOrCreate({ facebookId: profile.id }, function (err, user) {
      return cb(err, user);
    });
  }
));

app.get('/auth/facebook',
  passport.authenticate('facebook'));

app.get('/auth/facebook/secrets',
  passport.authenticate('facebook', { failureRedirect: '/login' }),
  function (req, res) {
    // Successful authentication, redirect secrets page.
    res.redirect('/secrets');
  });


//LOGIN USING GITHUB

passport.use(new GitHubStrategy({
  clientID: process.env.GITHUB_CLIENT_ID,
  clientSecret: process.env.GITHUB_SECRET,
  callbackURL: "http://localhost:3000/auth/github/secrets"
},
  function (accessToken, refreshToken, profile, done) {
    User.findOrCreate({ githubId: profile.id }, function (err, user) {
      return done(err, user);
    });
  }
));

app.get('/auth/github',
  passport.authenticate('github', { scope: ['user:email'] }));

app.get('/auth/github/secrets',
  passport.authenticate('github', { failureRedirect: '/login' }),
  function (req, res) {
    // Successful authentication, redirect secrets page.
    res.redirect('/secrets');
  });







app.get("/login", function (req, res) {
  res.render("login");
});
app.get("/register", function (req, res) {
  res.render("register");
});
app.get("/logout", function (req, res) {
  res.render("home");
});


app.get("/secrets",function(req,res){
  User.find({"secret":{$ne: null}})
  .then(foundUser=>{
    if(foundUser){
      res.render("secrets",{userwithsecrets:foundUser});
    }
    else{
      console.log('User secret cannot be published!!');
    }
  });
});

app.get("/submit", function (req, res) {
  if (req.isAuthenticated()) {
    res.render("submit");
  }
  else {
    res.redirect("/login");
  }
});

app.post("/submit", function (req, res) {
  const submittedsecret=req.body.secret;
  console.log(req.user.id);
  User.findById(req.user.id.toString())
  .then(foundUser => {
    if (foundUser) {
      foundUser.secret = submittedsecret;
      console.log("done !!!!!");
      foundUser.save()
        .then(() => res.redirect('/secrets'));
    } else {
      console.log('User not found');
    }
  })
  .catch(err => console.log(err));
});




app.get("/logout", function (req, res) {
  req.logout();
  res.redirect("/");
});

app.post("/register", function (req, res) {

  User.register({ username: req.body.username }, req.body.password, function (err, user) {
    if (err) {
      console.log(err);
      res.redirect("/register");
    }
    else {
      passport.authenticate("local")(req, res, function () {
        res.redirect("/secrets");
      });
    }
  })

});

app.post("/login", function (req, res) {

  const user = new User({
    username: req.body.username,
    password: req.body.password
  });
  req.login(user, function (err) {
    if (err) {
      console.log(err);
    }
    else {
      passport.authenticate("local")(req, res, function () {
        res.redirect("/secrets");
      });
    }
  })

});

app.listen(3000, function () {
  console.log("Server sarted on port 3000.");
})
```

