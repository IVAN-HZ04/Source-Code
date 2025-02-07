# Source-Code
Source Java code for Mobile Detailing Business

package com.mycompany.mobdetail;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import java.text.*;
import java.util.*;


//Class that handles appointment logic
class PlanDetail {
    //Arrays to store plan names and corresponding prices
    String[] plans = {"Neat Freak", "Economic", "Lite"};
    int[] prices = {100, 70, 50};  // Corresponding prices for each plan

    //Method to get the price based on selected plan
    public String getPrice(String selectedPlan) {
        for (int i = 0; i < plans.length; i++) {
            if (selectedPlan.equals(plans[i])) {
                return "Price: $" + prices[i];
            }
        }
        return "Invalid plan.";
    }

    //Method to check if the time is within business hours (9 AM to 3 PM)
    public boolean isBusinessHours(String timeInput) {
        SimpleDateFormat sdf = new SimpleDateFormat("hh:mm a");
        //Prevent invalid time format
        sdf.setLenient(false); 
        
        try {
            Date appointmentTime = sdf.parse(timeInput);
            Calendar calendar = Calendar.getInstance();
            calendar.setTime(appointmentTime);
            //24-hour format
            int hour = calendar.get(Calendar.HOUR_OF_DAY);  

            //Return true if the time is within business hours
            return hour >= 9 && hour <= 15;
        } catch (ParseException e) {
            return false;
        }
    }
}



//Subclass that extends PlanDetail and adds GUI functionality
public class MobDetail extends PlanDetail implements ActionListener {
    
    JLabel jlab, jtimeLab;
    JComboBox<String> comboBox;
    JTextField timeField;

    MobDetail() {
        //Create a new JFrame container
        JFrame jfrm = new JFrame("Appointment Verification");
        
        //Specify FlowLayout for the layout manager
        jfrm.setLayout(new FlowLayout());
        
        //Give the frame an initial size
        jfrm.setSize(300, 200);
        
        //Terminate the program when the user closes the application
        jfrm.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        //Create a JComboBox for selecting the plan, using the 'plans' array from PlanDetail
        comboBox = new JComboBox<>(plans);
        
        //Create a JTextField for user to input time
        jtimeLab = new JLabel("Enter Time (e.g., 10:00 AM): ");
        timeField = new JTextField(10);
        
        //Make "Accept" button
        JButton jbtnAcpt = new JButton("Accept");
        jbtnAcpt.addActionListener(this);
        
        //Add components to the content pane
        jfrm.add(comboBox);
        jfrm.add(jtimeLab);
        jfrm.add(timeField);
        jfrm.add(jbtnAcpt);
        
        //Create a label to show the message
        jlab = new JLabel("Select Plan and Click 'Accept'");
        jfrm.add(jlab);
        
        //Display the frame
        jfrm.setVisible(true);
    }
    
    //Handle button events
    public void actionPerformed(ActionEvent ae) {
        if (ae.getActionCommand().equals("Accept")) {
            String selectedPlan = (String) comboBox.getSelectedItem();
            String message = "";
            
            //Check the time entered by the user
            String timeInput = timeField.getText();
            
            if (isBusinessHours(timeInput)) 
                //Time is within business hours
                message = "Appointment Verified: " + getPrice(selectedPlan);
             else 
                //Time is outside business hours
                message = "Business hours are from 9 AM to 3 PM.";
            
            //Set the message on the label
            jlab.setText(message);
        }
    }
    
    public static void main(String[] args) {
        //Create the frame on the event dispatching thread
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                new MobDetail();
            }
        });
    }
}
