# MIBS: A New Lightweight Block Cipher
## Welcome to MIBS cipher official site

###Introduction
MIBS uses a Feistel structure with data block length of 64-bit and key lengths of 64-bit or 80-bit and consists of 32 rounds. The round structure is shown in Fig. 1. For applications that require moderate security levels, such as low-cost RFID tags, 64-bit security is adequate. In practice, there is a tradeoﬀ between hardware eﬃciency and security. The F-function, depicted in Fig. 2, operates on half a block (32 bits), representing it into eight nibbles, and it consists of four stages: key addition, non-linear substitution layer, linear mixing layer, and nibble-wise permutation.


Key addition. Current state $s31 , s30 , ..., s0$ , which is input to the F-function, is combined with a round subkey k i = k31 i , k30 i , . . . , k0i f or1 ≤ i ≤ 32, using a bit-wise XOR operation. Since XOR is well-suited to hardware implementation, all subkeys are bitwise XORed with data before substitution layer. sj = sj ⊕ kji , for0 ≤ j ≤ 31

You can use the [editor on GitHub](https://github.com/mibscipher/mibscipher.github.io/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.
$ x=y$
### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/mibscipher/mibscipher.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
