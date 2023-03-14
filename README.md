# practice-assessment



Vaccclinic.java

package vaccclinic;

/**
 *
 * @author Adie
 */
public class VaccClinic {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        // TODO code application logic here
        PQInterface pqInterface = new MyPQ();
        VaccGUI clinicApp = new VaccGUI();
        clinicApp.setVisible(true);
    }
    
}



Patient.java

package vaccclinic;

/**
 *
 * @author Adie
 */
public class Patient {
     private String patientName;
        private String patientAge;
        private String medicalCon;
        private String prior;

    public Patient() {
        patientName = new String();
        patientAge = new String();
        medicalCon = new String();
        prior = new String();
    }

    public String getPatientName() {
        return patientName;
    }

    public void setPatientName(String patientName) {
        this.patientName = patientName;
    }

    public String getPatientAge() {
        return patientAge;
    }

    public void setPatientAge(String patientAge) {
        this.patientAge = patientAge;
    }

    public String getMedicalCon() {
        return medicalCon;
    }

    public void setMedicalCon(String medicalCon) {
        this.medicalCon = medicalCon;
    }

    public String getPrior() {
        return prior;
    }

    public void setPrior(String prior) {
        this.prior = prior;
    }
    
   
    
}

PQElement.java

package vaccclinic;

/**
 *
 * @author Adie
 */
public class PQElement {
    private int iKey;
      private Patient patient;
     
      public PQElement(int iInPriority, Patient inPatient){
        iKey = iInPriority;
                patient = inPatient;
    }

      public int getiKey() {
        return iKey;
    }

      public void setiKey(int iInKey) {
        iKey = iInKey;
    }
    
    public Patient getPatient() {
            return patient;
      }

    public void setPatient(Patient inPatient) {
        patient = inPatient;
      }

      public String printPatient() {
          String Message ; 
          Message = "patient name = " + patient.getPatientName()+ " medical condition = " + patient.getMedicalCon();
          return Message;
      }

    String getElement() {
        throw new UnsupportedOperationException("Not supported yet."); // Generated from nbfs://nbhost/SystemFileSystem/Templates/Classes/Code/GeneratedMethodBody
    }
    
}


MyPQ.java

package vaccclinic;

import java.util.*;

/**
 *
 * @author Adie
 */
public class MyPQ implements PQInterface {

    private ArrayList<PQElement> priorQ;

    public MyPQ() {
        priorQ = new ArrayList<PQElement>();
    }

    public boolean isEmpty() {
        return priorQ.isEmpty();
    }

    public int size() {
        return priorQ.size();
    }

    //a new element with a given key and element information will be added 
    public void enqueue(int iPriorityKey, Object patient) {
        int iIndex;
        PQElement newElement = new PQElement(iPriorityKey, (Patient) patient);

        iIndex = findInsertPosition(iPriorityKey);

        if (iIndex == size()) {
            priorQ.add(newElement);
        } else {
            priorQ.add(iIndex, newElement);
        }
    }

    //find the position where the new element will be added to the list in descending order
    //based on the key (priority) provided
    private int findInsertPosition(int iNewKey) {
        boolean bFound = false;
        int iPosition = 0;
        PQElement curElement;
        while (iPosition < priorQ.size() && !bFound) {
            curElement = priorQ.get(iPosition);
            if (curElement.getiKey() > iNewKey) {
                iPosition = iPosition + 1;
            } else {
                bFound = true;
            }
        }
        return iPosition;
    }

    //remove the first element on the list, which has the highest key (priority)
    public Object dequeue() {
        return priorQ.remove(0);
    }

    public String printPQueue() {
        String printString = new String();
        PQElement curElement;
        for (int iCount = 0; iCount < priorQ.size(); iCount++) {
            curElement = priorQ.get(iCount);
            printString = printString.concat(curElement.printPatient() + " * Priority = " + curElement.getiKey() + "\n");
        }
        return printString;
    }

    @Override
    public String printPatient() {
        throw new UnsupportedOperationException("Not supported yet."); // Generated from nbfs://nbhost/SystemFileSystem/Templates/Classes/Code/GeneratedMethodBody
    }
    
    
}

VaccGUI.java (GUI source code)

private PQInterface myPQ;
    
    public VaccGUI(){
        myPQ = new MyPQ();
        initComponents();
    }

 private void addBtnActionPerformed(java.awt.event.ActionEvent evt) {                                       
        // TODO add your handling code here:
        String patientName, medicalCon, patientAge;
        int iPriority;
        Integer priorityINT;
        
        Patient newPatient = new Patient();
        patientName = nameTF.getText();
        medicalCon = mcTF.getText();
        patientAge = ageTF.getText();
        newPatient.setPatientName(patientName);
        newPatient.setMedicalCon(medicalCon);
        newPatient.setPatientAge(patientAge);
        
        priorityINT = Integer.valueOf(priorityTF.getText());
        iPriority = priorityINT.intValue();
        myPQ.enqueue(iPriority,newPatient);

        textArea.append(nameTF.getText() +" " +mcTF.getText()+" " +ageTF.getText()+" "+ " has been successfully added to the waiting list\n");
        nameTF.setText("");
        mcTF.setText("");
        ageTF.setText("");
        priorityTF.setText("");
    
    }                         


 private void displayBtnActionPerformed(java.awt.event.ActionEvent evt) {                                           
        // TODO add your handling code here:
        textArea.append("the full list of patients registered is: " + myPQ.size() +" patients !");
        //textArea.append("The patients on the waiting list are...\n");
        //textArea.append(myPQ.printPQueue()+ "\n");
        //textArea.append("There are " + myPQ.size() + " patients on the waiting list\n");
    }                                          

    private void nextGroupBtnActionPerformed(java.awt.event.ActionEvent evt) {                                             
        // TODO add your handling code here:
        if(!myPQ.isEmpty()){
            PQElement pqElement = (PQElement)myPQ.dequeue();
            Patient patient = (Patient)pqElement.getPatient();
            
            textArea.append("The GP will now serve group " + patient.getPatientName() +"\n");
            textArea.append("do they have a medical condition: " + patient.getMedicalCon()+ "\n");
            textArea.append("Their priority is: " + pqElement.getiKey()+ "\n");
        }
        else
            textArea.append("There are no patients waiting!\n");
    }                


