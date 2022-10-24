# CTF 2022 - Spanning Tree Qualifications: Bad Dice

## Description
![description](https://user-images.githubusercontent.com/111431985/197604705-e395f508-7c13-4c33-ae82-2c55a49b24b4.png)

## Attached files
- 9698dbdb5eba6643354e3bd731a282c8 (MD5) (REDACTED)

## Flag
```cyber_w1ldcard_f0r_unbound_re4d```

## Solution

First we had to connect via SSH to given address to receive our IP. Then we used local forwarding to connect to the game.

Game was about guessing random number between 0 and 666. After choosing number we were asked how many points we would like to bet. We started with 3. The goal of the 
game was to beat the high score, which was 13371337. Game was broken because it displayed correct number after each round. After playing few rounds, we finally beat the 
highscore and got the flag.

![result](https://user-images.githubusercontent.com/111431985/197606032-9c21b048-0268-451f-a879-d3114577a72b.jpg)
