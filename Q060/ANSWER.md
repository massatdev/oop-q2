# Q060 — Your answer

## (a) The trace

TODO(Q060): give the label and `handled` after each of the nine steps.

Answer:

Java:
0. start             handled=0  label: (no quote yet)
1. type 4.2          handled=0  label: (no quote yet)
2. click Quote       handled=1  label: WF 4.5 kg: Rp31,500, ready in 48 h
3. tick Express      handled=2  label: WF 4.5 kg express: Rp47,250, ready in 12 h
4. choose WDI        handled=3  label: WDI 4.5 kg express: Rp67,500, ready in 18 h
5. type abc          handled=3  label: WDI 4.5 kg express: Rp67,500, ready in 18 h
6. press Enter       handled=4  label: Weight must be a number
7. untick Express    handled=5  label: Weight must be a number
8. choose IR         handled=6  label: Weight must be a number
9. type 0.4, click   handled=7  label: IR 2.0 kg: Rp10,000, ready in 24 h

Python:
0. start             handled=0  label: (no quote yet)
1. type 4.2          handled=0  label: (no quote yet)
2. click Quote       handled=1  label: WF 4.5 kg: Rp31,500, ready in 48 h
3. tick Express      handled=2  label: WF 4.5 kg express: Rp47,250, ready in 12 h
4. choose WDI        handled=3  label: WDI 4.5 kg express: Rp67,500, ready in 18 h
5. type abc          handled=3  label: WDI 4.5 kg express: Rp67,500, ready in 18 h
6. press Enter       handled=4  label: Weight must be a number
7. untick Express    handled=5  label: Weight must be a number
8. choose IR         handled=6  label: Weight must be a number
9. type 0.4, click   handled=7  label: IR 2.0 kg: Rp10,000, ready in 24 h


## (b) The stale label

TODO(Q060): why does step 5 leave a label that contradicts the field, and what event would
you listen to in order to fix it?

Answer:
At step 5 the field contains "abc", but the label still shows the old quote (WDI 4.5 kg express, Rp67,500). A JTextField fires an ActionEvent only when Enter is pressed, not on each keystroke, and none of the four listeners observe the field's text. So typing triggers no listener, refresh never runs, and handled stays unchanged (it also stays at 0 after step 1). The label is a copy computed earlier, not linked to the field, so it goes stale. The same happens in the Tkinter version, because the controller only reacts to the events the window forwards, and typing forwards none.
To fix it, we need to listen to the text itself. In Swing, we can add a DocumentListener to the field's document, which fires on every insertion or removal of text, and call refresh from it. In Tkinter, we can use a trace on the StringVar or a <KeyRelease> binding. Pressing Enter (step 6) also corrects the label, but only after the user remembers to do it.