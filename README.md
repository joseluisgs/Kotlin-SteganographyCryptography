# Kotlin Steganography and Cryptography
Proyecto de evaluación para el título de Kotlin Developer en Jetbrains Academy. Consiste en esconder y cifrar mensajes en una imagen.

[![Kotlin](https://img.shields.io/badge/Code-Kotlin-blueviolet)](https://kotlinlang.org/)
[![LISENCE](https://img.shields.io/badge/Lisence-MIT-green)]()
![GitHub](https://img.shields.io/github/last-commit/joseluisgs/Kotlin-SteganographyCryptography)


![imagen](https://www.adesso-mobile.de/wp-content/uploads/2021/02/kotlin-einfu%CC%88hrung.jpg)

## Acerca de
Este proyecto de la academia Jetbrains fue realizado con el fin de evaluar la capacidad de desarrollo de Kotlin. Dada una imagen consiste en esconde y cifrar un mensaje en ella, y posteriormente descifrarlo.

- [Kotlin Steganography and Cryptography](#kotlin-steganography-and-cryptography)
  - [Acerca de](#acerca-de)
  - [Enunciado](#enunciado)
    - [Parte 1](#parte-1)
      - [Description](#description)
    - [Parte 2](#parte-2)
      - [Description](#description-1)
    - [Parte 3](#parte-3)
      - [Description](#description-2)
    - [Parte 4](#parte-4)
      - [Description](#description-3)
  - [Autor](#autor)
    - [Contacto](#contacto)
  - [Licencia](#licencia)

## Enunciado
### Parte 1
#### Description
A steganography/cryptography program is versatile: it can perform a variety of tasks based on the user's wishes. You may want to hide a message within an image, encrypt the text message for extra security, or decipher a hidden message you got from someone else.

In this stage, you will create the user interface for your program. The user will be able to choose a task by giving commands via standard input.

Objectives
The program should read input strings (commands) in a loop.

Your program should:

Print the message Task (hide, show, exit): and read standard input in a loop.
If the input is exit, print Bye! and exit.
If the input is hide, print Hiding message in image. and return to the input loop.
If the input is show, print Obtaining message from image. and return to the input loop.
If any other string is input, then print Wrong task: [input String] and return to the input loop.
Example
The greater-than symbol followed by a space (> ) represents the user input. Note that it's not part of the input.

Example: How the user interface should work.
```
Task (hide, show, exit):
> hide
Hiding message in image.
Task (hide, show, exit):
> show
Obtaining message from image.
Task (hide, show, exit):
> task
Wrong task: task
Task (hide, show, exit):
> exit
Bye!
```

### Parte 2
#### Description
Before we can get to actually concealing a message in an image, we need to learn how to handle images. In this stage, we will work on the command hide.

For our purposes, the image has to be in a lossless format. "Lossless" implies that the pixel values don't change when the image is saved and compressed. We are going to work with the PNG image format. Other known formats such as JPG use lossy compression image format, which would corrupt the hidden message.

Images can be viewed as 2-dimensional arrays. In the picture below, you can see the image coordinate system:

A 24-bit image offers more than 16 million colors for every pixel. If the RGB color scheme is used, then 8 bits (values 0–255) represent the Red, Green, and Blue colors. If the least significant bits of these 8-bit values change, the difference in the image will remain unnoticed. This is exactly what we will use for hiding our message in the next stages. In this stage, our program needs to set the least significant bit for each color of each pixel of the input image to the value 1.

Java classes BufferedImage, ImageIO, and Color (java.awt.image.BufferedImage, javax.imageio.ImageIO, and java.awt.Color) should be used for reading/writing and changing the images.

With the BufferedImage class methods, the image pixels can be accessed as a 2-dimensional array. Also, note that the type BufferedImage.TYPE_INT_RGB should be used for 24-bit color with the BufferedImage class.

You may find it helpful to look at some examples of the BufferedImage, ImageIO, and Color classes.

Finally, I/O operations can easily fail. However, the program should not stop when an I/O exception occurs, for example, if a wrong input filename is given, so all I/O exceptions should be handled.

Objectives
When the user inputs the command hide, the program should prompt them for an input image filename with the message Input image file: and an output image filename with the message Output image file:. These should be used for reading the input image file and writing the output image file, respectively.

After reading the filenames, the program should print the following messages: Input Image: [input filename] and Output Image: [output filename].

When the input image is read, the least significant bit for each color (Red, Green, and Blue) is set to 1. The resulting image will be saved with the provided output image filename in the PNG format.

A proper method should be applied so that the I/O exceptions do not terminate the program. In such cases, an exception message should be printed and the program should return to the command loop.


### Parte 3
#### Description
Steganography is about hiding information in such a way that no-one would ever guess there's a secret message hidden right before their eyes. The method we are going to use for concealing a message in an image is based on slight color changes that can’t be detected.

As you already know, the message data can be inserted at the positions of the least significant bits of each color value of each pixel. That makes 3 bits per pixel and a total of 3*[image width]*[image height] bits for the whole image. Of course, there is no need to use all of them: it would be more efficient to have an algorithm that picks which bits to use. We could make it even more complex: the bit selection can be based on a password so that the configuration is different every time the password is changed.

However, to keep things simple, in this project, we will use only the least significant bits of the blue color of each pixel. Thus, each image can hold up to [image width]*[image height] bits.

As seen in the previous stage, images can be handled as 2-dimensional arrays. In this stage, the order of pixels should be left to right in the first row and then the same for each consecutive row. The picture below illustrates the order:

The message to hide has the String type and UTF-8 charset. As a result, the message can be in any language. In order to conceal the secret message, we have to first convert it to an Array of Bytes: you can do it with the encodeToByteArray() function. Respectively, an array of bytes can be restored to the String type by applying toString(Charsets.UTF_8).

When the program reads the image bits in order to reconstruct the message, it has to know when to stop, that is, when it has read the entire message. For this, certain bytes should be applied at the end of the Bytes Array. Specifically, we will add three bytes with values 0, 0, 3 (or 00000000 00000000 00000011 in the binary format). When the program encounters these bytes, it will know that it has reached the end of the message.

Objectives
When the hide command is given, the program gets the input and output image filenames as in the previous stage. Then, it prompts the user for the secret message by printing Message to hide:.

The message should be converted to an array of bytes. Then, 3 bytes with the values 0, 0, 3 should be added at the end of the array.

The program should check that the image size is adequate for holding the Bytes array. If not, it should print an error message with the text The input image is not large enough to hold this message. and return to the menu.

Each bit of this Bytes Array will be saved at the position of the least significant bit of the blue color of each pixel, as shown in the picture below. The output image should be saved in the PNG format.



When the show command is given, the program asks for the image filename (previously saved with the hidden message) by printing Input image file:. The image will be opened and the Bytes Array will be reconstructed bit by bit; the program will stop reading it when the bytes with the values 0, 0, 3 are encountered.

The last 3 bytes (values 0, 0, 3) should be removed from the end of the Bytes Array. Then, the message should be restored as a String from the Bytes Array (or 00000000 00000000 00000011 bits).

The program should print Message: and then the message itself on a new line.



### Parte 4
#### Description
We hid the message well, but one can't be too careful, so let's encrypt the message itself.

Exclusive OR (XOR) is a logical operation whose output is only true when its inputs differ. XOR can also be used on data bits where 1 stands for true and 0 stands for false. Below you can find both the XOR truth table (on the left) with the input and output values of the XOR operation and the XOR bitwise operation (on the right).

XOR has an interesting mathematical feature: A XOR B = C and C XOR B = A. If A is the message and B the password, then C is the encrypted message. Using B and C, we can reconstruct A.

Below is an example where we use Bytes as a message, a password, and an encrypted message:

Thus, not only the same password can be used for encryption and decryption (symmetric encryption), but also the same simple method. This encryption is very strong if the password is random and has the same size as the message.

In this stage, the user should be asked for a password, and then the message is encrypted before it's hidden in the image.

Objectives
When the hide command is given and the secret message is input, the user should be prompted for a password with the message Password:.

The program reads the password string and converts it to a Bytes Array. The first message byte will be XOR encrypted using the first password byte, the second message byte will be XOR encrypted with the second password byte, and so on. If the password is shorter than the message, then after the last byte of the password, the first byte of the password should be used again.

Three Bytes with values 0, 0, 3 should be added to the encrypted Bytes Array. If the image size is adequate for holding the Bytes array, the result is hidden in the image like in the previous stage.

When the show command is given and the filename is input, the user should be prompted for the password with the message Password:. The image should open and the encrypted Bytes Array should be reconstructed just like in the previous stage; the program stops reading it when the bytes with the values 0, 0, 3 are found. The last three bytes should be removed and the encrypted Bytes Array should be decrypted using the password. Finally, the message should be restored to the String type, and the program should print Message: and then the message itself on a new line.

## Autor

Codificado con :sparkling_heart: por [José Luis González Sánchez](https://twitter.com/joseluisgonsan)

[![Twitter](https://img.shields.io/twitter/follow/joseluisgonsan?style=social)](https://twitter.com/joseluisgonsan)
[![GitHub](https://img.shields.io/github/followers/joseluisgs?style=social)](https://github.com/joseluisgs)

### Contacto
<p>
  Cualquier cosa que necesites házmelo saber por si puedo ayudarte 💬.
</p>
<p>
    <a href="https://twitter.com/joseluisgonsan" target="_blank">
        <img src="https://i.imgur.com/U4Uiaef.png" 
    height="30">
    </a> &nbsp;&nbsp;
    <a href="https://github.com/joseluisgs" target="_blank">
        <img src="https://distreau.com/github.svg" 
    height="30">
    </a> &nbsp;&nbsp;
    <a href="https://www.linkedin.com/in/joseluisgonsan" target="_blank">
        <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/LinkedIn_logo_initials.png/768px-LinkedIn_logo_initials.png" 
    height="30">
    </a>  &nbsp;&nbsp;
    <a href="https://joseluisgs.github.io/" target="_blank">
        <img src="https://joseluisgs.github.io/favicon.png" 
    height="30">
    </a>
</p>


## Licencia

Este proyecto está licenciado bajo licencia **MIT**, si desea saber más, visite el fichero [LICENSE](./LICENSE) para su uso docente y educativo.