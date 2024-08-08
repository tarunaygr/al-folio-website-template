---
layout: post
title: n00bzunit3d 2024 Rev/FlagChecker
date: 2024-08-05 
description: Writeup for Rev/FlagChecker in n00bzunit3d 2024
tags: n00bzunit3d2024
categories: writeup
---
Welcome to my series of writeups for n00bzunit3d 2024 capture-the-flag competition. In this post, we look at the Rev/FlagChecker challenge. This challenge gives us a `.xlsm` (MS Excel sheet) file and hints that the macros in it hold the information we need.

On opening the file and looking at the macros we see the following macro


{% highlight python linenos %}
Rem Attribute VBA_ModuleType=VBAModule
Option VBASupport 1
Sub FlagChecker()

    Dim chars(1 To 24) As String
    guess = InputBox("Enter the flag:")
    If Len(guess) <> 24 Then
        MsgBox "Nope"
    End If
    char_1 = Mid(guess, 1, 1)
    char_2 = Mid(guess, 2, 1)
    char_3 = Mid(guess, 3, 1)
    char_4 = Mid(guess, 4, 1)
    char_5 = Mid(guess, 5, 1)
    char_6 = Mid(guess, 6, 1)
    char_7 = Mid(guess, 7, 1)
    char_8 = Mid(guess, 8, 1)
    char_9 = Mid(guess, 9, 1)
    char_10 = Mid(guess, 10, 1)
    char_11 = Mid(guess, 11, 1)
    char_12 = Mid(guess, 12, 1)
    char_13 = Mid(guess, 13, 1)
    char_14 = Mid(guess, 14, 1)
    char_15 = Mid(guess, 15, 1)
    char_16 = Mid(guess, 16, 1)
    char_17 = Mid(guess, 17, 1)
    char_18 = Mid(guess, 18, 1)
    char_19 = Mid(guess, 19, 1)
    char_20 = Mid(guess, 20, 1)
    char_21 = Mid(guess, 21, 1)
    char_22 = Mid(guess, 22, 1)
    char_23 = Mid(guess, 23, 1)
    char_24 = Mid(guess, 24, 1)
    If (Asc(char_1) Xor Asc(char_8)) = 22 Then
        If (Asc(char_10) + Asc(char_24)) = 176 Then
            If (Asc(char_9) - Asc(char_22)) = -9 Then
                If (Asc(char_22) Xor Asc(char_6)) = 23 Then
                    If ((Asc(char_12) / 5) ^ (Asc(char_3) / 12)) = 130321 Then
                        If (char_22 = char_11) Then
                            If (Asc(char_15) * Asc(char_8)) = 14040 Then
                                If (Asc(char_12) Xor (Asc(char_17) - 5)) = 5 Then
                                    If (Asc(char_18) = Asc(char_23)) Then
                                        If (Asc(char_13) Xor Asc(char_14) Xor Asc(char_2)) = 121 Then
                                            If (Asc(char_14) Xor Asc(char_24)) = 77 Then
                                                If 1365 = (Asc(char_22) Xor 1337) Then
                                                    If (Asc(char_10) = Asc(char_7)) Then
                                                        If (Asc(char_23) + Asc(char_8)) = 235 Then
                                                            If Asc(char_16) = (Asc(char_17) + 19) Then
                                                                If (Asc(char_19)) = 107 Then
                                                                    If (Asc(char_20) + 501) = (Asc(char_1) * 5) Then
                                                                        If (Asc(char_21) = Asc(char_22)) Then
                                                                            MsgBox "you got the flag!"
                                                                        End If
                                                                    End If
                                                                End If
                                                            End If
                                                        End If
                                                    End If
                                                End If
                                            End If
                                        End If
                                    End If
                                End If
                            End If
                        End If
                    End If
                End If
            End If
        End If
    End If
End Sub
{% endhighlight %}

The macro checks if the input is exactly 24 characters long. If it is, it then proceeds to check a number of relationships between the ascii values of the input. If all the checks pass, we have the right flag value. All flags are of the form `n00bz{...}` so we already know some of the characters of the flag. By working backwards and finding the remaining characters that satify the relationships, we get the flag value.