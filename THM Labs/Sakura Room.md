# Writeup: Sakura Room - TryHackMe (OSINT)

## 🕵️‍♂️ Introduction
This is my walkthrough for the **Sakura Room** on [TryHackMe](https://tryhackme.com/room/sakura). This room is an excellent challenge for practicing Open Source Intelligence (OSINT) techniques on a specific target.

## 🛠️ Tools Used
During the investigation, I used the following tools and techniques:
* **Search Engines:** Google Dorking and Google Lens
* **Username Enumeration:** Sherlock
* **Social Media:** X (Twitter), LinkedIn, GitHub.
* **Cryptography & Keys:** Kleopatra (Gpg4win) – Used to import and inspect the PGP public key to uncover the attacker's email.
* **Cryptocurrency Analysis:** [Arkham](https://intel.arkm.com)
* **Geolocation & Mapping:** 
    * **Google Maps & Street View:** To verify physical landmarks and confirm locations.
    * **WiGLE (Wireless Geographic Logging Engine):** To map BSSIDs and pinpoint the geographic coordinates of wireless networks.

## 🚩 Walkthrough (Step-by-Step)
### Task 1: Introduction
In this write-up, I document my investigation of the **Sakura Room** lab on TryHackMe. This challenge simulates a real-world OSINT scenario in which my goal was to track down a cybercriminal using only source code and the clues the cybercriminal left behind.

### Task 2: TIP-OFF
The investigation started with an image file in which the attacker boasted about having hacked the dojo; however, in this case, the cybercriminal made a serious operational security mistake and was completely exposed, including his username.

From my browser, I simply clicked “View Source” on the image file and saw the attacker's source code and metadata, but what stood out most was their username, “SakuraSnowAngelAiko,” which was completely exposed in the file path.

![[SourceCode.png]]

### Task 3: 🔍 Reconnaissance 
The goal here is to find public information about the attacker "SakuraSnowAngelAiko" using their publicly available information.

**Solution:**

* **Step 1:** To learn more about this user, you can use a tool called Sherlock, which searches for usernames across hundreds of social media platforms. The command used was: sherlock SakuraSnowAngelAiko.
* **Step 2**: We will only take the Github link into account for the next steps, since the other users that appear on other social platforms are false positives, although the one from Telegram is not, but it has little important information for research.

![[SakuraSnowAngelAiko_ Bloc de notas 11_04_2026 06_46_44 p. m..png]]

* **Step 3:** To find this user’s email, we need to go to his Github, the one we previously found with Sherlock. Once inside, we must go to the repositories section and once there we must select the one called PGP and enter the file called publickey. 

![[PGP_publickey at main · sakurasnowangelaiko_PGP - Brave 11_04_2026 07_06_42 p. m..png]]

* **Step 4:** To decrypt that PGP key, a dedicated tool is needed; in our case we will use Kleopatra, once decrypted we will see an email linked from the cybercriminal, Her name is Aiko Abe.

![[Kleopatra 11_04_2026 07_26_04 p. m..png]]

### Task 4: 🔍 UNVEIL
In this task, the goal would be to track the cyberattacker's cryptocurrency transactions.

* **Step 1:** To find out which cryptocurrency the attacker has in their wallet, we need to go to the repositories and navigate to the section labeled “ETH.”
* **Step 2:** Now that we know the attacker is using the cryptocurrency Ethereum, we need to find their wallet address.
* **Step 3:** Well, to find the address of your wallet, it is very important within the ETH repository to see the commits, since there we will find the address. In this case, the address would be 0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef.

![[Create miningscript · sakurasnowangelaiko_ETH@5d83f7b - Brave 11_04_2026 10_13_07 p. m..png]]

* **Step 4:** To track transactions from the Monero address we obtained, we can use websites or tools designed for that purpose; in my case, I’ll use a crypto analysis website called Arkham.
* **Step 5:** Once on the website, enter the address into the search bar. To find the payments received on January 23, 2021 (UTC), filter by date to make it easier. As you can see, payments were received from a mining pool called Ethermine.

![[dsBuffer.bmp.png]]

* **Step 6:** The attacker used his wallet to exchange another cryptocurrency called Tether; the transaction is shown in the image below.

![[dsBuffer.bm.png]]

### Task 5: 🌐🔍 TAUNT
In this task our goal is to get the Twitter username (x) of the cybercriminal and especially the BSSID; here I will explain how I did it step by step.

* **Step 1:** To find the cybercriminal’s username, the key was to look at the screenshot we were given and note the username of the person who posted the image @AikoAbe3. We searched for that username on X (Twitter) and discovered the cybercriminal’s real username, @SakuraLoverAiko, through a mention they made of the user @AikoAbe3.

![[(14) @AikoAbe3 - Search _ X - Brave 12_04_2026 05_34_35 p. m..png]]

* **Step 2:** Looking at the attacker’s tweets, we realized he left very important clues about where he stores his Wi-Fi network information and password specifically on Deeppaste, which is hosted on the dark web.

![[(14) Aiko on X_ _Not too concerned about someone else finding them on the Dark Web. Anyone who wants them will have to do a real DEEP search to find where I PASTEd them._ _ X - Brave 12_04_2026 06_10_46 p. m..png]]

* **Step 3:** To identify where this information is located in Deeppaste, we need to look for the information shown in the image, since the image itself contains a specific name: “Regular WiFi and Passwords.” Additionally, it stores this information with a unique identifier, which in this case would be an MD5 hash.

![[FfFDMydaUAAVJPf.png]]

* **Step 4:** Once we've found the entry in deeppaste, we need to look up the attacker's home SSID, which in this case would be DK1F-G. 

![[deeppaste.png]]

**Note:**  The .onion link is no longer available; the only way I was able to access the image and thus the information was through the official OSINT DOJO repository. 

* **Step 5:** With that SSID, we should go to the advanced search in WiGLE and enter the SSID mentioned above, which will return the BSSID, which would be 84:af:ec:34:fc:f8

![[WiGLE search - Brave 12_04_2026 06_55_50 p. m..png]]

### Task 6: 🏠📍 HOMEBOUND
In this task, we need to determine the exact location of the city where the cybercriminal lives. Since he’s returning home, we realized he’d been traveling based on the images he posted on X (Twitter) and, most importantly, on the clues in his posts. Next, we’ll see how I connected the dots to pinpoint the location.

* **Step 1:** To determine which airport is closest to the location from which the attacker shared a photo before boarding his flight, we need to look at the obelisk visible in the background, a monument dedicated to Washington located in Washington, D.C., United States. If we triangulate the obelisk, the soccer field that can be seen, the water on the right, and the train tracks on the right side, we can geolocate the cybercriminal’s location and, therefore, determine which airport is closest to his area.

![[Esh-uTvUcAc-sXC.jpg]]

* **Step 2:** With the information we have so far, we would arrive at Long Bridge Park, confirm our location using the satellite view, and check the points mentioned earlier.

![[Long Bridge.png]]

* **Step 3:** The closest airport we found is Ronald Reagan Washington National Airport, and its code is DCA.

![[Ronald Reagan Washington National Airport - Google Maps - Brave 13_04_2026 05_10_28 p. m..png]]

![[Ronald Reagan Washington National Airport - Google Maps - Brave 13_04_2026 05_12_28 p. m..png]]

* **Step 4:** To find out which airport the attacker used for his last stopover, we need to look at the following post he made on X.

![[(14) Aiko on X_ _My final layover, time to relax! https___t.co_ZSdw9dLEH2_ _ X - Brave 13_04_2026 11_15_39 p. m. 1.png]]

* **Step 5:** After performing a reverse image search, we found the name of the airport, which is Haneda Airport. To confirm this, we looked it up on Google. Once confirmed, we also need to find the code, which is HND.

![[Buscar con Google - Brave 13_04_2026 11_31_33 p. m..png]]

![[_Haneda Airport_ - Search with Google - Brave 13_04_2026 11_51_03 p. m..png]]

* **Step 6:** To figure out which lake is shown on the map the attacker shared during his last flight home, we need to look at a few quick clues that will help us such as the shape of the river in the photo and, above all, start our search from the airport we found earlier.

![[(14) Aiko on X_ _Sooo close to home! Can't wait to finally be back! _) https___t.co_DZZwDziGkE_ _ X - Brave 14_04_2026 12_10_34 a. m..png]]

So, we should start by looking for the airport on Google Maps; we’ll need to scroll north to find it, and once we do, we’ll know it’s called Lake Inawashiro.

![[Haneda Airport - Google Maps - Brave 14_04_2026 12_26_36 a. m..png]]

* **Step 8:** To find out which city the cybercriminal considers safe, we need to look at the deep paste, but for now we should focus on the Mcdonalds, City Free Wifi, and Home Wifi networks and search the SSIDs in Wigle.

* **Step 9:** Once we've found the BSSIDs, we'll look up their location. As we can see, it's located in the city of Hirosaki, Japan.

![[Hirosaki - Google Maps - Brave 14_04_2026 06_10_48 p. m..png]]

  Finally, I’d like to thank everyone who has read this guide.











