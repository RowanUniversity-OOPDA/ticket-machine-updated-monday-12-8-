ticket-machine-updated-monday-12-8-
===================================
import java.util.ArrayList;
import java.util.Scanner;
import java.lang.Integer;

public class ticketmachine {
	double price;
	double balance;
	int amtFree;
	String membership;
	String to;
	String from;
	int quantity;
	boolean usefree;
	Membership mem = new Membership();
	private TrainList trainlist = new Trainlist();
	private Account acc = new Account();
	
	
	public static void main(String[] args) {
		ticketmachine machine = new ticketmachine();
		
		machine.useSubMachine();
	}
	public ticketmachine() {
		balance = 0;
		price =0;
		amtFree=0;
		quantity = 0;
		trainlist.loadTrains();
		
	}
	
	
	
	public void useSubMachine() {
		price = 5.00;//sets standard price as $5.
		amtfree= mem.returnTicketCount(); //allen's method that returns number of free tickets the current customer has
		membership = mem.checkMembership(); //allen's method that returns the type of membership this customer has
		subwayPrices(); //sets price based on membership
		checkquan();//if quantity > 1 then machine sets prices for other tickets and adds to the balance
		insertMoney();
	}
	
	public Train Schedule(String to, String from) { //dont use as fields
		ArrayList<Train> tl = trainlist.getTrains(to,from);
		if (tl.isEmpty () ) {
			System.out.println("No available trains between those stations.") ;
			return null;
		}
		else{
			int i =1;
			for (Train t:tl) {
				System.out.println(" ( "+ i + ")" + t.toString());
				i++;
			}
			System.out.println("Please select your train's number.");
			Scanner dest = new Scanner (System.in);
			String destination = dest.nextLine();
			int trainID = Integer.parseInt(destination);
			if (trainID >= 1 && trainID <= tl.size()) {
				return tl.get(trainID -1);
			}
			else{
				System.out.println("Invalid train number.");
				return null;
			}
		}
	}
	
	
	
	public String returnTo() {
		return to;
	}
	
	public String returnFrom() {
		return from;
	}
	
	
	
	 public void insertMoney() {
		 System.out.println("Tickets being purchased: " + quantity);
	       
		 while (balance > 0) {
			 System.out.println("Your balance is" + balance + ". To pay, please begin to insert money.");
		 
	        Scanner paid = new Scanner(System.in);
	        double inserted = paid.nextDouble();
	        balance = balance - inserted;
		 }
		 System.out.println("Your tickets are printing");
	    }
	
	
	public void useFreeTicket() {
		if (amtFree >0) {
			price=0;
			amtFree--;
		}
		else{
			System.out.println("Sorry, you have no free tickets available. ");
			usefree = false;
			subwayPrices();
			
		}
				
		
	}
	
	public void checkquan() {
		if (quantity > 0) {
            for(int i = 1; i < quantity; i++) {
                subwayPrices();
            }
		}
         
        else {
            balance = price;
        }
	}
	public void subwayPrices() {
		if (membership == "platinum") {
            price = 0.00; 
        }
        else if (membership == "gold") {
            
                price = price - (price * .5);
            
        }
        else if (membership == "silver" ) {
          
            
                price = price - (price * .25);
            
        }
		if(usefree) {
			useFreeTicket();
		}
		
	balance =+ price;
		
        
    }
	
	public void printTicket() {
		for(int i=1; i< quantity; i++) {
			System.out.println("###################");
			System.out.println("Your subway train ticket");
			System.out.println("To: " + to);
			System.out.println("From: " + from);
			System.out.println("Ticket price:" + price);
			System.out.println("Thank you for riding with us, please come again!");
			
			
		}
		
		
	}
	

}
