# Managing passwords

## Minimum Requirements

There are honestly too many secret management solutions for me to attempt going over them all. As such, there are a few conditions a prospective password manager must satisfy to be considered in this article.

First, the password manager must run on Linux. I run Linux. Our club is largely powered by Linux. If it doesn't work with Linux, it doesn't suit our needs.

Second, the password manager must either store password data locally or be self-hostable. LastPass **include link to lastpass leak here** has demonstrated why this is necessary.

Third, the password manager must be open source.

Fourth, the project needs to demonstrate a reasonable level of activity.

## The Competitors

### Honorable(?) mentions

There were a couple password managers I came across that met the minimum requirements, but I felt didn't fit me very well. They may work for you, but I didn't include them for one or more reasons.

The first such project was something called Titan Passwords. I say was in the past tense, because [something seems to have happened to that project](https://github.com/nrosvall/titan). I only found out about it thanks to an [opensource.com article](https://opensource.com/article/18/4/3-password-managers-linux-command-line). [It does seem to have been open source](https://ostechnix.com/titan-command-line-password-manager-linux/), but I don't think you should bother with a project that has evidently been abandoned.

The second such project is [pa](https://github.com/biox/pa). It's a very very simple project written in 130 lines of POSIX-compliant shell code. In my opinion it's a little... too simple.

bitwarden

https://github.com/buttercup

## Testing Criteria
- Code complexity
- Ease of use
- Command line interface
- Generating passwords
- Hides metadata
- TOTP
- History
- Synchronization
- Multiple users
- Multiple stores

### Testing methodology
0. Download code and measure complexity with scc
1. Create a new password store
    - Ease of use
    - Command line interface
2. Generate a new account
    - Metadata transparency
    - Password generation
3. Change the password
    - History
4. Add TOTP to account
5. Sync password store to another computer
6. Give permissions to another user
7. Create another password store and switch to it
8. Add a binary file to that store
