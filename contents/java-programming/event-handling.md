# Part 6 Asynchronous Event Handling

- Overview
- Register Event Handler
- MouseListener
- MouseAdapter
- Techniques for Event Handler
- Low Level Events
- High Level Events


---

# Overview
- Changing the state of an object is known as an event
- Step to perform Event Handling is register the component with the Listener

---

# Register Event Handler

- Instead, simply register a mouse event handler
```java
       addMouseListener(yourMouseListener);
```
- Java automatically starts a separate thread to look for events, and when a mouse
  event occurs, that thread automatically calls the appropriate methods in your handler
- Your handler must have all the MouseListener methods

---

# MouseListener
- Interface that defines 5 abstract methods: mouseEntered, mouseExited,
  mousePressed, mouseReleased, mouseClicked
  - Don’t use mouseClicked; it means a release
    where the mouse did not move “much”
    since it was pressed. Use mousePressed or mouseReleased instead
- Your listener class must implement all 5 methods

---

# MouseAdapter
- Abstract class that already implements the MouseListener interface and defines
  concrete versions of all 5 methods (with empty method bodies)
- Your listener class can extend MouseAdapter, override the method you care about,
  and ignore the others

---

# Techniques for Event Handler
- Using Separate Classes
- Main Class Implementing Interface
- Using Named Inner Classes
- Using Anonymous Inner Classes
- Using Lambdas

---

## Using Separate Classes

- Advantages
    - Can extend adapter and thus ignore unused methods
    - Can pass arguments to class constructor
    - Separate class is more reusable
- Disadvantages
    - Need extra step to call methods in main window, and those methods must be public
```java
          addMouseListener(new SeparateClass(...));
```

---

## Main Class Implementing Interface

- Advantages
    - Can easily call methods in main window, even private ones
- Disadvantages
    - Must implement methods you might not care about
    - Hard to have multiple different versions since you cannot pass arguments to listener
```java
          addMouseListener(this);
```

---

## Using Named Inner Classes

- Advantages
    - Can extend adapter and thus ignore unused methods
    - Can easily call methods in main app, even private ones
    - Can define constructor and pass arguments
- Disadvantage
    - A bit harder to understand
```java
          addMouseListener(new InnerClass(...));
```

---

## Using Anonymous Inner Classes

- Advantages
    - Same as named inner classes, but shorter
- Disadvantage
    - Harder to understand for beginners
    - Not reusable
    - No constructors
```java
          addMouseListener(new MouseAdapter() { ... });
```

---

## Using Lambdas

- Features
    - Full access to code from surrounding class
    - No confusion about meaning of “this”
    - Much more concise, succinct, and readable
    - Encourage a functional programming style
    - But no instance variables, so lambdas are not always better
    - You cannot use lambdas for **mouse listeners** because MouseListener has multiple methods,
    and lambdas can be used only for 1-method interfaces
    - You could use lambdas for **ActionListener**, which applies to push buttons
```java
          addActionListener(event -> doSomethingWith(event));
```

---

# Low Level Events
- Moving/Dragging mouse
    - Use **MouseMotionListener** or **MouseMotionAdapter**
    - mouseMoved: mouse moved while button was up
    - mouseDragged: mouse moved while button was down
- Typing on the keyboard
    - Use **KeyListener** or **KeyAdapter**
    - keyTyped: key was released for a printable character
    - Get String with String.valueOf(event.getKeyChar());
    - JPanel normally ignores keyboard events. If you want
      it to get them, you must do this:
      ```java
              setFocusable(true);
              requestFocusInWindow();
      ```

---

# High Level Events
- Buttons use **ActionListerner**
- Checkboxes, Radio Buttons, and others use **ItemListener**
- Text controls use **TextListener**
- Frames and dialog boxes use **WindowListener**
- JEditorPane (to render HTML) has **HyperLinkListener**

---

## Questions?

![alt text](images/mojo-question.png "Question")