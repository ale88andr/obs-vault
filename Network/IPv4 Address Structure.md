## **IPv4 Address Structure**

An IPv4 address is written using dotted decimal notation, but it is actually a 32-bit address. This gives us a total of about 4.29 billion possible addresses. We divide the 32-bit address into four 8-bit sections called octets. Then we convert each octet into a decimal value. This is called **dotted decimal notation**, and that's how we write IPv4 addresses. 

![[Pasted image 20240329104551.png]]

The range of an IPv4 address can be from four 0s (0.0.0.0) in dotted decimal notation to four 255s (255.255.255.255).

![[Pasted image 20240329104618.png]]

## **IPv4 Address and Subnet Mask**

An IPv4 address has two main parts:

- A network portion
- A host portion


![[Pasted image 20240329104635.png]]

The **subnet mask** (also called the **prefix length**) separates the network portion from the host portion of the IPv4 address. 

A subnet mask is 32 bits long. It has a group of 1s followed by a group of 0s. The 1s indicate the network portion of the IP address, and the 0s indicate the host portion. 

### **How to Write a Subnet Mask**

There are two ways to write a subnet mask: 

#### **Dotted Decimal Notation**

We can use dotted decimal notation, just like for IPv4 addresses. 

Example: 255.255.0.0

#### **Slash Notation or Prefix Length**

We can also use a slash notation, which shows the number of 1 bits in the mask. 

Example:

- /16 (indicating 16 one bits)
- 172.18.0.0/16

Using the slash notation if more common nowadays. 

### **Finding the IPv4 Network Address Using the Subnet Mask**

Let’s say we have a host IPv4 address (192.168.1.100) and a subnet mask (255.255.255.0). How do we find the network address from the host address using the subnet mask? 

In binary notation, we perform a bitwise AND operation between the host address and the subnet mask. For each bit position, if both the address and mask bits are 1, the result is 1. Otherwise, the result is 0. The result gives us the network address.

![[Pasted image 20240329104703.png]]
