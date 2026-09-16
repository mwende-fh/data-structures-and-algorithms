# PARKING SYSTEM ALGORITHM
<!So i have to divide the whole thing to modules, i think i want to divide it into entry and exit but they also can be divided further>
<!i will divide it into : slot display, slot allocation, payment and exit(slot freeing)>


# MODULE 1 : SLOT DISPLAY
<! A car comes in we have to check if there's space for it to park>
input : status of the parking slots
output : display of all slots whether occupied or available
START
1. Retrieve the slots from the database table
2.Check each slots marking : occupied or available (checking their status)
For  = i to N
IF slotarray[i].status == Available then render slot i green with label "available"
ELSE render slot i as red with label slotarray[i].platenumber
END

# MODULE 2 : SLOT ALLOCATION
<!So after checking about the slots now its time to allocate, if>0 they can park if<1 no parking also if the number plate already is parked then reject, and also time to store info about the car in the system>
START
1. Enter the vehicle's plate number and details
2. Check if there's any slot available IF not then reject and display "parking full"
3. Check at the database table to see ELSE IF the vehicle is alreasy registered(is already parked/activesessions) if in the system then reject display "vehicle already parked"
4. ELSE allocate the vehicle a slot and reduce the count of slots 
5. Record the entry time 
6. Create session record (plate, slotnumber, entry time, exit time(null), fee(null))
7. persist to database
8. markthe slot occupied
END


# MODULE 3 : PAYMENT
<! Time to exit so we calculate the duration the vehicle has stayed in the park and the fee to be paid>
START
1. Record exit time which is equal to the current time
2. Calculate the duration = exit time - entry time (in minutes)
3. Retrieve the parking rats from the database
4. Check the rates from the lowest duration to the highest
5. Find the first rate where duration<=maximum minutes in each tier
6. Set the fee to that rate
7. Store and display the fee 
END


# MODULE 4 : EXIT
<! confirm if they have paid and then open the barrier and increase the slot number>
START
1. Display the fee to the barrier
2. Check whwther the driver has paid and the payment is successful
3. Compare the amount paid with the amount due
4. IF amount paid < amount due
      then display "Insufficient payment" keep the barrier closed and allow the driver to make another payment 
5. ELSE IF amount paid > amount due
      then record the payment, calculate the change and display it to the driver, return the change and open the barrier
6. ELSE amount paid = amount due
      record the payment then display "payment successful" and no change and open the barrier
7. Remove the vehicle from the list of active sessions in the database
8. Set the slot as free and increase the slot counts by 1
END