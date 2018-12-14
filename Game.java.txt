import java.util.*;
class Room {

public String getName() {
return name;
}

public void setName(String name) {
this.name = name;
}

public String getDescription() {
return description;
}

public void setDescription(String description) {
this.description = description;
}

public Room(String name) {
setName(name);
}
  
@Override
public String toString() {
return name;
}

private String name;
private String description;
}

//**Main.java**



enum Commands {
UNKNOWN,
QUIT,
LOOK,
NORTH,
SOUTH,
EAST,
WEST
}


public class Main {

public static void main(String[] args) {
System.out.println("Welcome to Zork!");
  
  
InitializeRoomDescriptions();
  
SpawnPlayer();
  
Room currentRoom = rooms[locationX][locationY];
Room previousRoom = currentRoom;
  
Scanner scanner = new Scanner(System.in);

Commands command = Commands.UNKNOWN;
  
while (command != Commands.QUIT) {
System.out.println(currentRoom);
System.out.print(">");
String inputString = scanner.nextLine();
command = ToCommand(inputString);

switch (command)
{
case QUIT:
System.out.println("Thank you for playing!");
break;

case LOOK:
System.out.println(currentRoom.getDescription());
break;

case NORTH:
case SOUTH:
case EAST:
case WEST:
  
Move(command);
currentRoom = rooms[locationX][locationY];
if (currentRoom != previousRoom) {
System.out.println(currentRoom.getDescription());
previousRoom = currentRoom;
}
break;

default:
System.out.println("Unknown command.");
break;
}
}
}

private static Commands ToCommand(String commandString) {
try {
return Commands.valueOf(commandString.trim().toUpperCase());
} catch (IllegalArgumentException ex) {
return Commands.UNKNOWN;
}
}

private static void Move(Commands command) {
if (IsDirection(command) == false)
{
throw new IllegalArgumentException();
}

boolean isValidMove = false;
switch (command)
{
  
case NORTH:
if (locationY < rooms[locationX].length - 1) {
isValidMove = true;
locationY++;
}
break;

case SOUTH:
if (locationY > 0) {
isValidMove = true;
locationY--;
}
break;

case EAST:
if (locationX < rooms.length - 1) {
isValidMove = true;
locationX++;
}
break;

case WEST:
if (locationX > 0) {
isValidMove = true;
locationX--;
}
break;
}

if (isValidMove == false)
{
System.out.println("The way is blocked!");
}
}

private static boolean IsDirection(Commands command) {
return directions.contains(command);
}
  
private static void SpawnPlayer(){
Random random = new Random();
locationX = random.nextInt(rooms.length);
locationX = random.nextInt(rooms[0].length);
}
private static void InitializeRoomDescriptions() {
rooms[0][0].setDescription("There's a coat hook on the wall.");
rooms[0][1].setDescription("You see a tidy, cozy kitchen");
rooms[0][2].setDescription("You see a table set for dinner.");
rooms[1][0].setDescription("You are in the living room.");
rooms[1][1].setDescription("You see a bed, a dresser, and a mirror.");
rooms[1][2].setDescription("You are in the bathroom. You see a toilet and a vanity.");
rooms[2][0].setDescription("There is chess set on a table with two chairs on either side.");
rooms[2][1].setDescription("You see a washer and dryer");
rooms[2][2].setDescription("A rubber may saying 'Welcome to Zork!' lies by the door.");
}
  
private static Room[][] rooms = {
{ new Room("Foyer"), new Room("Kitchen"), new Room("Dining Room") },
{ new Room("Living Room"), new Room("Bedroom"), new Room("Bathroom ") },
{ new Room("Game Room"), new Room("Laundry"), new Room("Porch") }
};
  
private static int locationX = 0;
private static int locationY = 0;
  
private static final ArrayList directions = new ArrayList(Arrays.asList(
Commands.NORTH,
Commands.SOUTH,
Commands.EAST,
Commands.WEST));
}