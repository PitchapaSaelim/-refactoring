# Refactoring

## Dragon Tiger Game
*From Pitchapa's Dragon Tiger Game code: https://github.com/PitchapaSaelim/pa4-PitchapaSaelim*

### handle() method in class ChangeChipsHandler

In the `src/dragontiger/GameUI.java` class

https://github.com/PitchapaSaelim/pa4-PitchapaSaelim/blob/master/src/dragontiger/GameUI.java

consider this code: 

```java
class ChangeChipsHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        .
        .
        .
            if (type == yesButton) {
                chooseSides = 99;
                TextInputDialog inputUserChips = new TextInputDialog(null);
                inputUserChips.setTitle("✎ Change user's chips");
                inputUserChips.setHeaderText("Please enter an Integer Number");
                inputUserChips.setContentText("Enter your chips:");
                inputUserChips.showAndWait();
                String getUserChips = inputUserChips.getResult();               
```
* Refactoring Signs: 
    - == operator is for reference comparison(address comparison).
    - == operator checks if both objects point to the same memory location.

* Refactoring: replace == operator with .equals() method. (Also refactoring handle() method in class OpenCardHandler and class NewGameHandler.)
    - .equals() method evaluates to the comparison of values in the objects.
```java
class ChangeChipsHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        .
        .
        .
            if (type.equals(yesButton)) {                                              
                chooseSides = 99;                                                 
                TextInputDialog inputUserChips = new TextInputDialog(null);       
                inputUserChips.setTitle("✎ Change user's chips");                 
                inputUserChips.setHeaderText("Please enter an Integer Number");   
                inputUserChips.setContentText("Enter your chips:");               
                inputUserChips.showAndWait();                                     
                String getUserChips = inputUserChips.getResult();                 
```

### handle() method in class ConfirmHandler
In the `src/dragontiger/GameUI.java` class

https://github.com/PitchapaSaelim/pa4-PitchapaSaelim/blob/master/src/dragontiger/GameUI.java

consider this code: 

```java
public void handle(ActionEvent actionEvent) {
    .
    .
    .
    Alert alert = new Alert(Alert.AlertType.NONE);
    if (getTextBets.isEmpty()) {
        inputBets.setStyle("-fx-text-box-border: red");
        alert.setAlertType(Alert.AlertType.ERROR);
        alert.setTitle("Error");
        alert.setHeaderText(null);
        alert.setContentText("Please fill in information.");
        alert.showAndWait();
    }
    if (!getTextBets.isEmpty()) {
        if (!scanText.hasNextInt()) {
            try {
                userBets = Integer.parseInt(getTextBets);
            } catch (NumberFormatException nfe) {
                inputBets.setStyle("-fx-text-box-border: red");
                alert.setAlertType(Alert.AlertType.ERROR);
                alert.setTitle("Error");
                alert.setHeaderText(null);
                alert.setContentText("Not an Integer Number!");
                alert.showAndWait();
            }
        } else if (!(chooseSides == 0 || chooseSides == 1 || chooseSides == 2)) {
            inputBets.setStyle("-fx-text-box-border: red");
            alert.setAlertType(Alert.AlertType.ERROR);
            alert.setTitle("Error");
            alert.setHeaderText(null);
            alert.setContentText("Please Choose dragon OR tiger OR draw.");
            alert.showAndWait();
        } else {
            try {
                userBets = Integer.parseInt(getTextBets);
                if ((userBets > userChips) || (userBets <= 0)) {
                    inputBets.setStyle("-fx-text-box-border: #ffd811");
                    alert.setAlertType(Alert.AlertType.WARNING);
                    alert.setTitle("Sorry");
                    alert.setHeaderText(null);
                    alert.setContentText("You have lost all your chips.");
                    alert.showAndWait();
                    passConfirm = false;
                } else {
                    inputBets.setStyle(null);
                    inputBets.setStyle("-fx-text-box-border: #2c5df1");
                    alert.setAlertType(Alert.AlertType.INFORMATION);
                    alert.setTitle("Success");
                    alert.setHeaderText(null);
                    alert.setContentText("Bet Successful!");
                    alert.showAndWait();
                    passConfirm = true;
                }
            } catch (Exception e) {
                inputBets.setStyle("-fx-text-box-border: red");
                alert.setAlertType(Alert.AlertType.ERROR);
                alert.setTitle("Error");
                alert.setHeaderText(null);
                alert.setContentText("Error message!");
                alert.showAndWait();
            }
        }
    }
}
``` 
* Refactoring Signs: 
    - duplicate alert method.

* Refactoring: replace alert method with extract method. (Also refactoring handle() method in class OpenCardHandler, class ChangeChipsHandler, class NewGameHandler, and class HowToPlayHandler.)
    - create alert() method for creating alert boxes.
    - to reduce duplicate code and make it easier to understand.

```java
public void alert(Alert.AlertType alertType, String title, String header, String message) {
    Alert alert = new Alert(Alert.AlertType.NONE);
    alert.setAlertType(alertType);
    alert.setTitle(title);
    alert.setHeaderText(header);
    alert.setContentText(message);
    alert.showAndWait();
}

class ConfirmHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        .
        .
        .
        if (getTextBets.isEmpty()) {
            inputBets.setStyle("-fx-text-box-border: red");
            alert(Alert.AlertType.ERROR, "Error", null, "Please fill in information." );
        }
        if (!getTextBets.isEmpty()) {
            if (!scanText.hasNextInt()) {
                try {
                    userBets = Integer.parseInt(getTextBets);
                } catch (NumberFormatException nfe) {
                    inputBets.setStyle("-fx-text-box-border: red");
                    alert(Alert.AlertType.ERROR, "Error", null, "Not an Integer Number!");
                }
            } else if (!(chooseSides == 0 || chooseSides == 1 || chooseSides == 2)) {
                inputBets.setStyle("-fx-text-box-border: red");
                alert(Alert.AlertType.ERROR, "Error", null, "Please Choose dragon OR tiger OR draw.");
            } else {
                try {
                    userBets = Integer.parseInt(getTextBets);
                    if ((userBets > userChips) || (userBets <= 0)) {
                        inputBets.setStyle("-fx-text-box-border: #ffd811");
                        alert(Alert.AlertType.WARNING, "Sorry", null, "You have lost all your chips.");
                        passConfirm = false;
                    } else {
                        inputBets.setStyle(null);
                        inputBets.setStyle("-fx-text-box-border: #2c5df1");
                        alert(Alert.AlertType.INFORMATION, "Success", null, "Bet Successful!");
                        passConfirm = true;
                    }
                } catch (Exception e) {
                    inputBets.setStyle("-fx-text-box-border: red");
                    alert(Alert.AlertType.ERROR, "Error", null, "Error message!");
                }
            }
        }
    }  
}    
```

### handle() method in class ChooseDragonHandler, ChooseTigerHandler, and ChooseDrawHandler
In the `src/dragontiger/GameUI.java` class

https://github.com/PitchapaSaelim/pa4-PitchapaSaelim/blob/master/src/dragontiger/GameUI.java

consider this code: 
```java
.
.
.
public Label text;
.
.
.
class ChooseDragonHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        chooseDragon();
        text.setText("DRAGON");
        text.setStyle("-fx-text-fill: #ff0000");
        display();
    }
}


class ChooseTigerHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        chooseTiger();
        text.setStyle("-fx-text-fill: #5745e5");
        text.setText("TIGER");
        display();
    }
}

class ChooseDrawHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        chooseDraw();
        text.setStyle("-fx-text-fill: #92e75c");
        text.setText("DRAW");
        display();
    }
}
```
* Refactoring Signs: 
    - duplicate text method.

* Refactoring: replace text method with extract method.
    - create text() method for creating label text.
    - to reduce duplicate code and make it easier to understand.

```java
.
.
.
public Label text;
.
.
.
public void text(String style, String text) {
    text.setStyle(style);
    text.setText(text);
}

class ChooseDragonHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        chooseDragon();
        text("DRAGON", "-fx-text-fill: #ff0000");
        display();
    }
}

class ChooseTigerHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        chooseTiger();
        text("TIGER", "-fx-text-fill: #5745e5");
        display();
    }
}

class ChooseDrawHandler implements EventHandler<ActionEvent> {

    @Override
    public void handle(ActionEvent actionEvent) {
        chooseDraw();
        text("DRAW", "-fx-text-fill: #92e75c");
        display();
    }
}
```
