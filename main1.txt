package application;
	

import java.io.*;
import java.net.*;
import java.util.Date;
import javafx.application.Application;
import javafx.application.Platform;
import javafx.scene.Scene;
import javafx.scene.control.ScrollPane;
import javafx.scene.control.TextArea;
import javafx.stage.Stage;
public class Main extends Application {

  
  public String IsItPrime(int prime, int i, String j) {
	  boolean flag = false;
      for (i = 2; i <= prime / 2; ++i) {
        // condition for non prime number
        if (prime % i == 0) {
          flag = true;
          break;
          
        }
        else if(flag == true) {
        	j = "is Prime";
        
        }
        else {
        	j = "Not prime";
        }
      }
	  
	  return j;
	  
  }
  public void start(Stage primaryStage) {
    // Text area for displaying contents
    TextArea ta = new TextArea();
    // Create a scene and place it in the stage
    Scene scene = new Scene(new ScrollPane(ta), 450, 200);
    primaryStage.setTitle("Server"); // Set the stage title
    primaryStage.setScene(scene); // Place the scene in the stage
    primaryStage.show(); // Display the stage
    
    new Thread( () -> {
      try {
        // Create a server socket
        ServerSocket serverSocket = new ServerSocket(8000);
        Platform.runLater(() ->
          ta.appendText("Server started at " + new Date() + '\n'));
  
        // Listen for a connection request
        Socket socket = serverSocket.accept();
  
        // Create data input and output streams
        DataInputStream inputFromClient = new DataInputStream(
          socket.getInputStream());
        DataOutputStream outputToClient = new DataOutputStream(
          socket.getOutputStream());
  
        while (true) {
          // Receive radius from the client
          int prime = inputFromClient.readInt();
  
          
          // Send area back to the client
          outputToClient.writeChars(IsItPrime(prime, prime, null));
          
          Platform.runLater(() -> {
            ta.appendText("Number received from client: " 
              + prime + '\n');
            ta.appendText("The number is: " +IsItPrime(prime, prime, null)  + '\n'); 
          });
        }
      }
      catch(IOException ex) {
        ex.printStackTrace();
      }
    }).start();
  }
  /**
   * The main method is only needed for the IDE with limited
   * JavaFX support. Not needed for running from the command line.
   */
  public static void main(String[] args) {
    launch(args);
  }
}