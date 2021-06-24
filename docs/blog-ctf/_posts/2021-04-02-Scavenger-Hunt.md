---
layout: post
title:  "Scavenger Hunt | PicoCTF 2021"
date:   2021-04-02 13:03:00 +0800
categories: CTF PicoCTF Web
---

## Scavenger Hunt
#### Category: Web Exploitation

Landed on a HTTP site given. From here investigated the HTML of the page and found this part towards the bottom, commented out:
`<!-- Here's the first part of the flag: picoCTF{t -->`

After clicking on the `What` page, says HTML, CSS and JS were used. Looked at the html again and find `mycss.css` used. Visit here and find the next part of the flag:
`Here's part 2: h4ts_4_l0 */`

Visited some other files. But then thought about `robots.txt`. visited here and found the third part.
`# Part 3: t_0f_pl4c`


Also found a hint. `# I think this is an apache server... can you Access the next flag?`. Tried different directories for apache but realised it is probably a file already in the web file. so `.htaccess`. This found the next flag:
`Part 4: 3s_2_lO0k`

This also came up with a hint `I love making websites on my Mac, I can Store a lot of information there.`. Did some investigation online and from details with the MAC storage system know of files in directories called `.DS_Store` which sometimes appears.
Checked this out and found the next part:

`Part 5: _35844447}`


Put it all together and flag is 
`picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_35844447}`