# Pure Bash Rock Paper Scissors

Using nested for loops creates a grid

e.g.

```bash
list=(rock paper scissors)

for x in "${list[@]}"; do for y in "${list[@]}"; do echo "$x, $y"; done; done

rock, rock
rock, paper
rock, scissors
paper, rock
paper, paper
paper, scissors
scissors, rock
scissors, paper
scissors, scissors
```

By the way, never use nested for loops in production, it's too slow.

In the game code below

First declare the rules to the game (rules), but get them in to the right order of the grid output

Then use the nested for loops to create all the possible options (combinations)

I forgot how this code works on the combinations part exactly, (probably should have used an associative array, but "it works" and this is bash, so if it works, then it is acceptable. Buhaahaha.) But basically after that just generate a random computer choice and also take an input for a user choice. Compare the computers and human against the rule grid to output the correct outcome.

It all turns in to something like as follows, without any tedious if,then,else logic:

```bash
#!/bin/bash

# deps: bash, shuf, grep, cut, fontawesome

main () {
    play () {
        [ "$1" = d ] && { echo DYNAMITE IS CHEATING!; echo EXITING PROGRAM! CHEATER!; exit 1; }

        game=(rock paper scissors)

        rules=(" NPC:  YOU:  RESULT: ROCK = ROCK, YOU TIED!"
               " NPC:  YOU:  RESULT: PAPER > ROCK, YOU WIN!"
 " NPC:  YOU:  RESULT: ROCK > PAPER, YOU LOSE!"
               " NPC:  YOU:  RESULT: PAPER > ROCK, YOU LOSE!"
               " NPC:  YOU:  RESULT: PAPER = PAPER, YOU TIED!"
               " NPC:  YOU:  RESULT: SCISSORS > PAPER, YOU WIN!"
               " NPC:  YOU:  RESULT: ROCK > SCISSORS, YOU WIN!"
               " NPC:  YOU:  RESULT: SCISSORS > PAPER, YOU LOSE!"
               " NPC:  YOU:  RESULT: SCISSORS = SCISSORS, YOU TIED!")

        for x in "${game[@]}"; do
            for y in "${game[@]}"; do
                combinations+=("$x$y-${rules[0]}")
                rules=("${rules[@]:1}")
            done
        done

        npc="$(echo -e "rock\npaper\nscissors" | shuf -n 1;)"
        printf "%s \n" "${combinations[@]}" | grep "$npc$1" | cut -d '-' -f 2
        combinations=() ; game=() ; rules=()

        return 0
    }

    while :; do
        read -p "(r)ock, (p)aper, (s)cissors? ---- (q)uit " CHOICE
        case "$CHOICE" in
            r|p|s|d) play $CHOICE ;;
            q) exit 0 ;;
            *) echo "Invalid option: $CHOICE" ; continue ;;
        esac
    done
}

main
````

As it turns out, someone on the internet told me the game could be summarized in only one line of code,

```bash
echo -e "rock\npaper\nscissors" | shuf -n 1
```

PR accepted if someone wants to add a netcat http server and use Bash's ability to tap in to `/dev/tcp/host/port` for multiplayer support.
