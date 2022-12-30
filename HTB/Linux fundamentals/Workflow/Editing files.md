There are several ways to edit a file. One of the most common text editors for this is `Vi` and `Vim`. More rarely, there is the `Nano` editor. We will first deal with the Nano editor here, as it is a bit easier to understand. We can create a new file directly with the Nano editor by specifying the file's name directly as the first parameter. In this case, we create a new file named `notes.txt`.

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ nano notes.txt
```

Now we should see a so-called "`pager`" open, and we can freely enter or insert any text. Our shell should then look something like this.

#### Nano Editor

  Nano Editor

```shell-session
  GNU nano 2.9.3                                    notes.txt                                              

Here we can type everything we want and make our notes.▓


^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Spell    ^_ Go To Line  M-E Redo
```

Below we see two lines with short descriptions. The `caret` (`^`) stands for our "`[CTRL]`" key. For example, if we press `[CTRL + W]`, a "`Search:`" line appears at the bottom of the editor, where we can enter the word or words we are looking for. If we now search for the word "`we`" and press `[ENTER]`, the cursor will move to the first word that matches.

  Nano Editor

```shell-session
GNU nano 2.9.3                                    notes.txt                                              

Here ▓we can type everything we want and make our notes.

Search:   notes                                                                                            
^G Get Help    M-C Case Sens  M-B Backwards  M-J FullJstify ^W Beg of Par  ^Y First Line  ^P PrevHstory
^C Cancel      M-R Regexp     ^R Replace     ^T Go To Line  ^O End of Par  ^V Last Line   ^N NextHstory
```

To jump to the next match with the cursor, we press `[CTRL + W]` again and confirm with `[ENTER]` without any additional information.

  Nano Editor

```shell-session
GNU nano 2.9.3                                    notes.txt                                              

Here we can type everything ▓we want and make our notes.

Search [we]:                                                                                               
^G Get Help    M-C Case Sens  M-B Backwards  M-J FullJstify ^W Beg of Par  ^Y First Line  ^P PrevHstory
^C Cancel      M-R Regexp     ^R Replace     ^T Go To Line  ^O End of Par  ^V Last Line   ^N NextHstory
```

Now we can save the file by pressing `[CTRL + O]` and confirm the file name with `[ENTER]`.

  Nano Editor

```shell-session
GNU nano 2.9.3                                    notes.txt                                              

Here we can type everything we want and make our notes.

File Name to Write: notes.txt▓                                                                           
^G Get Help    M-C Case Sens  M-B Backwards  M-J FullJstify ^W Beg of Par  ^Y First Line  ^P PrevHstory
^C Cancel      M-R Regexp     ^R Replace     ^T Go To Line  ^O End of Par  ^V Last Line   ^N NextHstory
```

After we have saved the file, we can leave the editor with `[CTRL + X]`.