# flight

1. Write a query to display the average monthly ticket cost for each flight in ABC  Airlines. The query should display
the Flight_Id,From_location,To_Location,Month Name as “Month_Name” and average price as “Average_Price”. Display the records sorted
in ascending order based on flight id and then by Month Name.

A) SELECT f.flight_id,f.from_location,f.to_location,
monthname(af.flight_departure_date) Month_Name,
AVG(price) Average_Price FROM air_flight f JOIN air_flight_details af
ON f.flight_id = af.flight_id WHERE f.airline_name = 'abc'
GROUP BY f.flight_id,f.from_location,f.to_location,Month_Name 
ORDER BY f.flight_id, Month_Name;

--------------------------------------------------------------------------------------------------------------------------------------

2.Write a query to display the number of flight services between locations in a month. The Query should display From_Location, 
To_Location, Month as “Month_Name” and number of flight services as “No_of_Services”.  Hint: The Number of Services can be calculated
from the number of scheduled departure dates of a flight. The records should be displayed in ascending order based on From_Location and
then by To_Location and then by month name.

A) SELECT f.from_location,f.to_location,
monthname(af.flight_departure_date) Month_Name,
count(af.flight_departure_date) No_of_Services 
FROM air_flight f JOIN air_flight_details af
ON f.flight_id = af.flight_id
GROUP BY f.from_location,f.to_location,Month_Name
ORDER BY f.from_location,f.to_Location,Month_Name;

-------------------------------------------------------------------------------------------------------------------------------------

3.Write a query to display the customer(s) who has/have booked least number of tickets in ABC Airlines. 
The Query should display profile_id, customer’s first_name, Address and Number of tickets booked as “No_of_Tickets”Display 
the records sorted in ascending order based on customer's first name.

A) SELECT ap.profile_id,ap.first_name,ap.address,count(ati.ticket_id) No_of_Tickets FROM
air_passenger_profile ap JOIN air_ticket_info ati ON ap.profile_id=ati.profile_id
JOIN air_flight af ON af.flight_id=ati.flight_id and af.airline_name='abc'
GROUP BY ap.profile_id,ap.first_name,ap.address HAVING count(ati.ticket_id)<=ALL
(SELECT count(ticket_id)
FROM air_ticket_info GROUP BY profile_id)
ORDER BY ap.first_name;

----------------------------------------------------------------------------------------------------------------------------

4. Write a query to display the number of tickets booked from Chennai to Hyderabad. 
The Query should display passenger profile_id,first_name,last_name, Flight_Id , Departure_Date and  number of tickets booked as 
“No_of_Tickets”.Display the records sorted in ascending order based on profile id and then by flight id and then by departure date.

A) SELECT ap.profile_id,ap.first_name,ap.last_name,af.flight_id,ati.flight_departure_date,
count(ati.profile_id) No_of_Tickets FROM 
air_ticket_info ati JOIN air_passenger_profile ap ON ap.profile_id=ati.profile_id
JOIN air_flight af ON af.flight_id=ati.flight_id
WHERE af.from_location='Chennai' and af.to_location='Hyderabad'
GROUP BY ati.flight_id,ati.profile_id
ORDER BY ap.profile_id,af.flight_id,ati.flight_departure_date;

-----------------------------------------------------------------------------------------------------------------------------------

5. Write a query to display flight id,from location, to location and ticket price of flights whose departure 
is in the month of april.Display the records sorted in ascending order based on flight id and then by from location.

A) SELECT af.flight_id,af.from_location,af.to_location,afd.price FROM
air_flight af JOIN air_flight_details afd ON af.flight_id=afd.flight_id
and month(afd.flight_departure_date)='04'
ORDER BY af.flight_id,af.from_location;

-------------------------------------------------------------------------------------------------------------------------------

6. Write a query to display the average cost of the tickets in each flight on all scheduled dates. 
The query should display flight_id, from_location, to_location and Average price as “Price”. 
Display the records sorted in ascending order based on flight id and then by from_location and then by to_location.

A) SELECT af.flight_id,af.from_location,af.to_location,avg(afd.price) Average_Price FROM
air_flight af JOIN air_flight_details afd ON af.flight_id=afd.flight_id
GROUP BY af.flight_id
ORDER BY af.flight_id,af.from_location,af.to_location;

---------------------------------------------------------------------------------------------------------------------------------

7. Write a query to display the customers who have booked tickets from Chennai to Hyderabad. 
The query should display profile_id, customer_name (combine first_name & last_name with comma in b/w), address 
of the customer. Give an alias to the name as customer_name.Hint: Query should fetch unique customers irrespective of 
multiple tickets booked.Display the records sorted in ascending order based on profile id.

A) SELECT ap.profile_id,concat(ap.first_name,',',ap.last_name) customer_name,ap.address FROM
air_passenger_profile ap JOIN air_ticket_info ati ON ap.profile_id=ati.profile_id
JOIN air_flight af ON af.flight_id=ati.flight_id
WHERE af.from_location='Chennai' and af.to_location='Hyderabad'
GROUP BY ati.profile_id
ORDER BY ap.profile_id;

------------------------------------------------------------------------------------------------------------------------------------


8. Write a query to display profile id of the passenger(s) who has/have booked maximum number of 
tickets.In case of multiple records, display the records sorted in ascending order based on profile id.

A) SELECT profile_id FROM air_ticket_info
group by profile_id
having count(ticket_id)>=all(select count(ticket_id)
from air_ticket_info
group by profile_id) order by profile_id;

-------------------------------------------------------------------------------------------------------------------------------------

9. Write a query to display the total number of tickets as “No_of_Tickets” booked in each flight in ABC Airlines. 
The Query should display the flight_id, from_location, to_location and the number of tickets. Display only the flights in
which atleast 1 ticket is booked.Display the records sorted in ascending order based on flight id.

A) SELECT f.flight_id,f.from_location,f.to_location,COUNT(t.ticket_id) AS No_of_Tickets
FROM air_ticket_info t JOIN air_flight f
ON  f.flight_id = t.flight_id where AIRLINE_NAME = 'abc' GROUP by 
f.flight_id,f.from_location,f.to_location
having count(t.ticket_id)>=1
ORDER by f.flight_id;

-------------------------------------------------------------------------------------------------------------------------------------

10. Write a query to display the no of services offered by each flight and the total price of the services.
The Query should display flight_id, number of services as “No_of_Services” and the cost as “Total_Price” in the same order. 
Order the result by Total Price in descending order and then by flight_id in descending order.Hint:The number of services can be
calculated from the number of scheduled departure dates of the flight

A) SELECT flight_id,count(flight_departure_date) No_of_services,sum(price) Total_Price FROM
air_flight_details GROUP BY flight_id
ORDER BY Total_price DESC,flight_id DESC;

------------------------------------------------------------------------------------------------------------------------------------

11. Write a query to display the number of passengers who have travelled in each flight in each scheduled date.
The Query should display flight_id, flight_departure_date and the number of passengers as “No_of_Passengers” in the same order.
Display the records sorted in ascending order based on flight id and then by flight departure date.

A) SELECT flight_id,flight_departure_date,count(ticket_id) No_of_passengers FROM
air_ticket_info GROUP BY flight_id,flight_departure_date
ORDER BY flight_id,flight_departure_date;

-------------------------------------------------------------------------------------------------------------------------------------

12. Write a query to display profile id of passenger(s) who booked minimum number of tickets. 
In case of multiple records, display the records sorted in ascending order based on profile id.

A) SELECT profile_id FROM air_ticket_info
GROUP BY profile_id HAVING count(ticket_id)<=ALL
(SELECT count(ticket_id) FROM air_ticket_info GROUP BY profile_id)
ORDER BY profile_id;

------------------------------------------------------------------------------------------------------------------------------------

13. Write a query to display unique passenger profile id, first name, mobile number and email address of 
passengers who booked ticket to travel from HYDERABAD to CHENNAI.Display the records sorted in ascending order based on profile id.

A) SELECT DISTINCT ap.profile_id,ap.first_name,ap.mobile_number,ap.email_id FROM 
air_passenger_profile ap JOIN air_ticket_info ati ON ap.profile_id=ati.profile_id
JOIN air_flight af ON ati.flight_id=af.flight_id
WHERE af.from_location='Hyderabad' and af.to_location='Chennai'
ORDER BY profile_id;

--------------------------------------------------------------------------------------------------------------------------------------

14. Write a query to intimate the passengers who are boarding Chennai to Hyderabad Flight on 6th May 2013
stating the delay of 1hr in the departure time. The Query should display the passenger’s profile_id, first_name,last_name,
flight_id, flight_departure_date,  actual departure time , actual arrival time , delayed departure time as "Delayed_Departure_Time",
delayed arrival time as "Delayed_Arrival_Time" Hint: Distinct  Profile ID should be displayed irrespective of multiple tickets 
booked by the same profile.Display the records sorted in ascending order based on passenger's profile id.

A) SELECT DISTINCT ap.profile_id,ap.first_name,ap.last_name,ati.flight_id,ati.flight_departure_date,
af.departure_time,af.arrival_time,
addtime(af.departure_time,'01:00:00') Delayed_Deparuture_Time,
addtime(af.arrival_time,'01:00:00') Delayed_Arrival_Time FROM
air_passenger_profile ap JOIN air_ticket_info ati ON ap.profile_id=ati.profile_id
JOIN air_flight af ON af.flight_id=ati.flight_id
WHERE af.from_location='Chennai' and af.to_location='Hyderabad' 
and ati.flight_departure_date='2013-05-06'
ORDER BY profile_id;

---------------------------------------------------------------------------------------------------------------------------------------

15. Write a query to display the number of tickets as “No_of_Tickets” booked by Kochi Customers. 
The Query should display the Profile_Id, First_Name, Base_Location and number of tickets booked.Hint: Use String functions 
to get the base location of customer from their Address and give alias name as “Base_Location”Display the records sorted in
ascending order based on customer first name.

SELECT ap.profile_id,ap.first_name,
substring_index(substring_index(ap.address,'-',2),'-',-1) Base_Location,
count(ati.ticket_id) No_of_Tickets FROM 
air_passenger_profile ap JOIN air_ticket_info ati ON ati.profile_id=ap.profile_id
WHERE ap.address LIKE '%Kochi%'
ORDER BY ap.first_name;

--------------------------------------------------------------------------------------------------------------------------------------

16. Write a query to display the flight_id, from_location, to_location, number of Services as “No_of_Services” offered in 
the month of May.

A) SELECT af.flight_id,af.from_location,af.to_location,count(afd.flight_departure_date) No_of_services FROM
air_flight af JOIN air_flight_details afd ON af.flight_id=afd.flight_id
WHERE month(flight_departure_date)='05' 
GROUP BY af.flight_id,af.from_location,af.to_location
ORDER BY af.flight_id;

-----------------------------------------------------------------------------------------------------------------------------------

17. Write a query to display profile id,last name,mobile number and email id of passengers whose base location 
is chennai.Display the records sorted in ascending order based on profile id.

A) SELECT profile_id, last_name, mobile_number, email_id
FROM air_passenger_profile
WHERE address LIKE '%Chennai%' 
ORDER BY profile_id;

-----------------------------------------------------------------------------------------------------------------------------------

18. Write a query to display number of flights between 6.00 AM and 6.00 PM from chennai. Hint Use FLIGHT_COUNT as alias name.

A) SELECT count(flight_id) FLIGHT_COUNT FROM air_flight 
WHERE from_location='CHENNAI' 
AND departure_time BETWEEN '06:00:00' AND '18:00:00';

-------------------------------------------------------------------------------------------------------------------------------------

19. Write a query to display unique profile id,first name , email id and contact number of passenger(s)
who travelled on flight with id 3178. Display the records sorted in ascending order based on first name.

A) SELECT DISTINCT ap.profile_id,ap.first_name,ap.email_id,ap.mobile_number FROM 
air_passenger_profile ap JOIN air_ticket_info ati ON ap.profile_id=ati.profile_id
WHERE ati.flight_id='3178'
ORDER BY ap.first_name;

-------------------------------------------------------------------------------------------------------------------------------------

20. Write a query to display flight id,departure date,flight type of all flights.
Flight type can be identified based on the following rules : if ticket price is less than 3000 then 
'AIR PASSENGER',ticket price between 3000 and less than 4000 'AIR BUS' and ticket price between 4000 and greater than
4000 then 'EXECUTIVE PASSENGER'. Hint use FLIGHT_TYPE as alias name.Display the records sorted in ascendeing order based on 
flight_id and then by departure date.

A) SELECT flight_id,flight_departure_date,
case when price<3000 then 'AIR PASSENGER'
	when price>=3000 and price<4000 then 'AIR BUS'
	when price>=4000 then 'EXECUTIVE PASSENGER'
end FLIGHT_TYPE FROM air_flight_details
ORDER BY flight_id,flight_departure_date;

-----------------------------------------------------------------------------------------------------------------------------------

21. Write a query to display the credit card type and no of credit cards used on the same type. 
Display the records sorted in ascending order based on credit card type.Hint: Use CARD_COUNT AS Alias name for no of cards.

A) SELECT card_type, count(card_type) Card_Count FROM air_credit_card_details
GROUP BY card_type ORDER BY card_type;

--------------------------------------------------------------------------------------------------------------------------------

22.Write a Query to display serial no, first name, mobile number, email id of all the passengers who holds email address from gmail.com.
The Serial No will be the last three digits of profile ID.Hint: Use SERIAL_NO as Alias name for serial number.Display the records sorted in ascending order based on name.

A) SELECT substring(profile_id,-3) SERIAL_NO,first_name,mobile_number,email_id FROM
air_passenger_profile
WHERE email_id LIKE '%@gmail.com'
ORDER BY first_name;

------------------------------------------------------------------------------------------------------------------------------------

23.  Write a query to display the flight(s) which has least number of services in the month of May.
The Query should fetch flight_id, from_location, to_location, least number of Services as “No_of_Services”
Hint: Number of services offered can be calculated from the number of scheduled departure dates of a flight if there are multiple 
flights, display them sorted in ascending order based on flight id.

A) SELECT afd.flight_id,af.from_location,af.to_location,count(afd.flight_id) No_of_Services
FROM air_flight_details afd JOIN air_flight af ON af.flight_id=afd.flight_id
WHERE monthname(afd.flight_departure_date)='May'
GROUP BY afd.flight_departure_date HAVING count(afd.flight_id) <=
ALL(SELECT count(flight_id) FROM air_flight_details
WHERE monthname(flight_departure_date)='May'
GROUP BY flight_departure_date)
ORDER BY flight_id;

------------------------------------------------------------------------------------------------------------------------------------

24. Write a query to display the flights available in Morning, AfterNoon, Evening& Night. 
The Query should display the Flight_Id, From_Location, To_Location , Departure_Time, time of service as "Time_of_Service".
Time of Service should be calculated as: From 05:00:01 Hrs to 12:00:00 Hrs -  Morning, 12:00:01 to 18:00:00 Hrs -AfterNoon, 
18:00:01 to 24:00:00 - Evening and 00:00:01 to 05:00:00 - NightDisplay the records sorted in ascending order based on flight id.

A)  SELECT flight_id,from_location,to_location,Departure_Time,
CASE
WHEN departure_time BETWEEN ('05:00:01') AND ('12:00:00')
THEN 'Morning'
WHEN departure_time BETWEEN ('12:00:01') AND ('18:00:00')
THEN 'AfterNoon'
WHEN departure_time BETWEEN ('18:00:01') AND ('24:00:00')
THEN 'Evening'
WHEN departure_time='00:00:00'
THEN 'Evening'
WHEN departure_time BETWEEN ('00:00:01') AND ('05:00:00')
THEN 'Night'
END Time_of_Service
FROM air_flight
order by flight_id;

-------------------------------------------------------------------------------------------------------------------

25. Write a query to display the number of flights flying from each location. 
The Query should display the from location and the number of flights to other locations as “No_of_Flights”.
Hint: Get the distinct from location and to location.Display the records sorted in ascending order based on from location.

A) SELECT from_location,count(flight_id) No_of_Flights FROM
air_flight GROUP BY from_location
ORDER BY from_location;

--------------------------------------------------------------------------------------------------------------------------------------

26.  Write a query to display the number of passengers traveled in each flight in each scheduled date.
The Query should display flight_id,from_location,To_location, flight_departure_date and the number of passengers as 
“No_of_Passengers”. Hint: The Number of passengers inclusive of all the tickets booked with single profile id.Display 
the records sorted in ascending order based on flight id and then by flight departure date.

A) SELECT af.flight_id,af.from_location,af.to_location,ati.flight_departure_date,
count(ati.ticket_id) No_of_Passengers FROM
air_flight af JOIN air_ticket_info ati ON af.flight_id=ati.flight_id
GROUP BY af.flight_id,af.from_location,af.to_location,ati.flight_departure_date
ORDER BY af.flight_id,ati.flight_departure_date;

-------------------------------------------------------------------------------------------------------------------------------------

27. Write a query to display the flight details in which more than 10% of seats have been booked. 
The query should display Flight_Id, From_Location, To_Location,Total_Seats, seats booked as “No_of_Seats_Booked” 
.Display the records sorted in ascending order based on flight id and then by No_of_Seats_Booked.

A) SELECT af.flight_id,af.from_location,af.to_location,af.total_seats,
(af.total_seats-afd.available_seats) No_of_Seats_Booked FROM 
air_flight_details afd JOIN air_flight af ON afd.flight_id=af.flight_id
WHERE (af.total_seats-afd.available_seats)>(af.total_seats*0.1)
ORDER BY flight_id,No_of_Seats_Booked;

-----------------------------------------------------------------------------------------------------------------------------------

28. Write a query to display the Flight_Id, Flight_Departure_Date, From_Location,To_Location and Duration 
of all flights which has duration of travel less than 1 Hour, 10 Minutes.

A) SELECT af.flight_Id,afd.flight_Departure_Date,af.From_Location,af.To_Location,af.duration 
FROM air_flight af JOIN air_flight_details afd ON af.flight_id=afd.flight_id
WHERE af.duration<'01:10:00';

--------------------------------------------------------------------------------------------------------------------------------


29. Write a query to display the flight_id, from_location,to_location,number of services as “No_of_Services” , 
average ticket price as “Average_Price” whose average ticket price is greater than the total average ticket cost of  
all flights. Order the result by lowest average price.

A)  SELECT afd.flight_id,af.from_location,af.to_location,
count(afd.flight_departure_date) No_of_Service, avg(price) Average_Price 
FROM air_flight af JOIN air_flight_details afd ON af.flight_id=afd.flight_id
GROUP BY af.flight_id,af.from_location,af.to_location
HAVING avg(price)>(SELECT avg(price) FROM air_flight_details)
ORDER BY average_price;

------------------------------------------------------------------------------------------------------------------------------------
















