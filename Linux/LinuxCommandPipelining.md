# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is pipelining?

Pipelining allows the output of one command to be used as the input of another command. This is done using the pipeline operator '|'.

# Usage

Lets start from the basics. Suppose the command is as follows:
    cat file.txt | head
The cat comand shows us whats written in the file. The head command gives the first 10 lines. Of course, this particular example is redundant as doing head file.txt will give us the same output. But when you look at the flow of data, it goes from
    file -> cat -> head
instead of
    file -> head

Lets go a bit deeper into this.
Suppose we run:
    ps aux | grep ssh
This command first lists all the running processes in the system. Then through a pipeline, grep filters the output and shows the lines which contain the text/String 'ssh'.

A little more complex now.
    sudo journalctl -u ssh --no-pager | grep "Accepted" | wc -l    
This is a very powerful command chain. What we are essentailly doing here is as follows:
1. We are running the command journalctl via root user, which shows us the log files.
2. Using '-u ssh' we are essentially showing only the entries associated with the ssh systemd service.
3. We are filtering the log outputs and showing the ones which contain the text "Accepted", that is, we are showing Successful ssh logins recorded in the log.
4. Finally, we are counting the number of lines

What this command chain essetially does is that it counts the total number of successful ssh logins. Something like this is very important for an soc Analyst to understand.

We can also modify the command chain to count successful ssh logins grouped by the username:
    sudo journalctl -u ssh --no-pager | grep "Accepted" | awk -F' ' '{print $4}' | sort | uniq -c


# Flow

    A | B | C

means

    A's output -> B's input -> B's output -> C's input

