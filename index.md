<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
});
</script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# MIBS: A New Lightweight Block Cipher
## Welcome to MIBS cipher site

## Introduction

MIBS uses a Feistel structure with data block length of 64-bit and key lengths of 64-bit or 80-bit and consists of 32 rounds. The round structure is shown in Fig. 1. For applications that require moderate security levels, such as low-cost RFID tags, 64-bit security is adequate. In practice, there is a tradeoﬀ between hardware eﬃciency and security. The F-function, depicted in Fig. 2, operates on half a block (32 bits), representing it into eight nibbles, and it consists of four stages: key addition, non-linear substitution layer, linear mixing layer, and nibble-wise permutation.


**Key addition.** Current state \\(s_{31} , s_{30} , ..., s_{0}\\) , which is input to the F-function, is combined with a round subkey \\(k^i = k_{31}^i , k_{30}^i , ... , k_0^i\\) for \\(1 ≤ i ≤ 32\\), using a bit-wise XOR operation. Since XOR is well-suited to hardware implementation, all subkeys are bitwise XORed with data before substitution layer. 
       $$s_j = s_j \oplus k_j^ii , \mbox{ for } 0 ≤ j ≤ 31$$

**Substitution layer S.** After adding subkey, the block is divided into eight nibbles \\(x_8 , x_7 , ..., x_1\\) , before processing by the S-boxes. The 4 × 4 S-box used in our cipher is the same as the ﬁrst S-box used in mCRYPTON and is shown in Table [tab-mibs-sbox]. 

| \\(x\\)    | 0 | 1  | 2 | 3 | 4  | 5  | 6  | 7 | 8  | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
|------------|---|----|---|---|----|----|----|---|----|---|----|----|----|----|----|----|
| \\(S(x)\\) | 4 | 15 | 3 | 8 | 13 | 10 | 12 | 0 | 11 | 5 | 7  | 14 | 2  | 6  | 1  | 9  |
[S-box mapping][tab-mibs-sbox]

The non-linear layer is composed of eight identical 4 × 4 S-boxes, so in this transformation nibble-wise substitution is applied. 

$$S : F_2^4 → F_2^4 : x_i → y_i = s(x_i) , \mbox{ for } 1 ≤ i ≤ 8$$ 

**Mixing layer M.** The linear transformation mixes eight nibbles as follows: 

$$M : (GF(2)^4)^8 → (GF(2)^4)^8 , (y_8 , y_7 , . . . , y_1 ) → (y_8' , y_7' , . . . , y_1' ) ⇔$$

$$
\begin{array}{l}
y1' = y2 \oplus y3 \oplus y4 \oplus y5 \oplus y6 \oplus y7 \\
y2' = y1 \oplus y3 \oplus y4 \oplus y6 \oplus y7 \oplus y8 \\
y3' = y1 \oplus y2 \oplus y4 \oplus y5 \oplus y7 \oplus y8 \\
y4' = y1 \oplus y2 \oplus y3 \oplus y5 \oplus y6 \oplus y8 \\
y5' = y1 \oplus y2 \oplus y4 \oplus y5 \oplus y6 \\
y6' = y1 \oplus y2 \oplus y3 \oplus y6 \oplus y7 \\
y7' = y2 \oplus y3 \oplus y4 \oplus y7 \oplus y8 \\
y8 = y1 \oplus y3 \oplus y4 \oplus y5 \oplus y8 \\
\end{array}
$$

  **Permutation layer P.** Finally, the eight nibble outputs from the mixing layer are arranged according to Table [tab-mibs-perm]. Each nibble is moved to a new position by P. 


| /// | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-----|---|---|---|---|---|---|---|---|
| P   | 2 | 8 | 1 | 3 | 6 | 7 | 4 | 5 |
[Permutation mapping][tab-mibs-perm]

**Key schedule for 64-bit key.** The design principle of MIBS key schedule is adopted from the design principle of PRESENT key schedule. Our key schedule, generates 32-bit round key $k_i$ , for $ 0 ≤ i ≤ 31$ , from 64-bit user key $K$ (represented as $k_{63} , k_{62} , ..., k_0$ ). We denote the key state of the $i$-th round as state $i$ . The key state for each round is updated as follows. 

$$
\begin{array}{l}
state^0 = user-key \\
state^i = state^i \ggg 15 \\
state^i = S-box(state^i[63:60] )||state^i[59:0]\\
state^i = state^i[63:16] ||state^i[15:11] \oplus Round-Counter||state^i[10:0]\\
k^i = state^i[63:32] \\
\end{array}
$$

where $\ggg$ means rotation to right, $[i : j]$ indicates the $i$-th to the $j$-th bits are involved in the operation, and $||$ denotes concatenation. Also we use the same S-box as in the F-function. The round key $k^i$ is the 32 left most bits of the current state. 

**Key schedule for 80-bit key.** The key $K$ is ﬁrst initialized with the user key, and updates as follows. 

$$
\begin{array}{l}
state^0 = user-key \\
state^i = state^i \ggg 19 \\
state^i = S-box(state^i[79:76] )||S-box(state^i[75:72] )||state^i[71:0] \\
state^i = state^i[79:19] ||state^i[18:14] \oplus Round-Counter||state^i[13:0]\\
k^i = state^i[79:48]\\
\end{array}
$$

After that, the round key $k^i$ is the 32 left most bits of the key state.

## Design Rationale 
### The Cipher Structure 

MIBS is based on Feistel structure with an SPN round function. A large propor- tion of block ciphers have used this scheme since the US Federal Government adopted the DES. Moreover, DES has endured various attacks for over 20 years, even though its round function is very simple. Since Feistel construction oper- ates on half of the block length in each iteration, therefore the size of code or circuitry required to implement it is nearly halved. Thus we use Feistel network as an overall structure with the purpose of minimizing computational resources, which certainly is one of the most important considerations in hardware design for tiny ubiquitous devices. 

### Round Function 

For round function we selected the Substitution-Permutation Network (SPN). The SPN structure is directly based on the concepts of confusion and diﬀusion. The confusion component is a nonlinear substitution and the diﬀusion compo- nent is a linear mixing which is used for diﬀusing the cryptographic characteris- tics of substitution layer. The substitution layer. The most important objective in designing a block cipher targeted to embedded applications such as RFID tags, is to achieve low complexity in hardware while providing suﬃcient security. Consequently an ap- propriate substitution layer of such a block cipher should meets the above bal- ance. Although, large S-boxes can achieve better security but even in software, large S-boxes require high storage cost and they are far worse in hardware. On the other hand, too small S-boxes can hardly achieve suitable security. We observed the gate count increases exponentially with the size of S-box. As a result, we decided to use 4 × 4 S-boxes with regard to hardware efficiency and at the same time adequate security. Also existing lightweight block ciphers like PRESENT, and mCRYPTON have used 4 × 4 S-boxes too. The S-box used in MIBS block cipher is the same as the S0 mapping applied in mCRYPTON [7]. The linear transformation. In order to construct a fast and strong block cipher, we design a round function that is secure against diﬀerential and lin- ear cryptanalysis and yield small values for the maximum diﬀerential and linear probabilities p, q. Kanda et al. [11], proposed a search algorithm for construct- ing an optimal linear transformation layer by using the matrix representation in order to minimize probabilities p, q as much as possible. They determined an optimal linear transformation layer among many candidates which has a lower computational complexity, which we used in MIBS. Additionally they showed that any linear transformation following a non-linear layer consists of 8 parallel S-boxes, can not have branch number more than 5. The branch number is the minimum number of active S-boxes in two consecutive rounds of a non-trivial diﬀerential characteristic or non-trivial linear trail [12]. In this context, by Optimal we mean that the maximum diﬀerential and linear probabilities p, q are as small as possible. Similar linear transformations is used also in E2[13] and Camellia[14] block ciphers. The linear layer M, which we call mixing layer, is represented using only 16 nibble-wise XORs that is suitable for computational eﬃciency. For security against diﬀerential and linear cryptanalysis, the branch number of layer M is optimal. Consequently, the mixing layer piles up the num- ber of active S-boxes every two rounds to minimize the maximum diﬀerential and linear probabilities.

You can use the [editor on GitHub](https://github.com/mibscipher/mibscipher.github.io/edit/master/index.md) to maintain and preview the content for your website in Markdown files.


Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.


 
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
