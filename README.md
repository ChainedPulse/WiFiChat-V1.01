# WiFiChat-V1.01
Made In DroidScript

//Code//

function OnStart(){
//Start Debug
app.CreateDebug();

//Set Orientation
app.SetOrientation( "Portrait" );

//Create UDP network object.
net = app.CreateNetClient( "UDP" );
	
//Get the UDP broadcast address and set our port number.
address = net.GetBroadcastAddress();
port = 19700;
	
//Get our MAC address (to serve as a device id).
mac = app.GetMacAddress();
	
//Start timer to check for incoming messages.
setInterval( CheckForMsg, 200 );

//Layouts
lay1 = app.CreateLayout( "Linear", "VerticalFillXY" );
lay1.SetBackColor( "#ff505050" );

//Send Button
sendbtn = app.CreateButton( "Send", 1 );
sendbtn.SetTextSize( 25 );
sendbtn.SetTextColor( "#ffffff" );
sendbtn.SetOnTouch( sendMsg );

//Username Edit
useredt = app.CreateTextEdit( "", 1 );
useredt.SetTextSize( 25 );
useredt.SetTextColor( "#ffffff" );
useredt.SetHint( "Username" );

//Send Text Edit
sendedt = app.CreateTextEdit( "", 1 );
sendedt.SetTextSize( 25 );
sendedt.SetTextColor( "#ffffff" );
sendedt.SetHint( "Message" );

//Adding Layouts
app.AddLayout( lay1 );

//Adding Children
lay1.AddChild( sendbtn );
lay1.AddChild( useredt );
lay1.AddChild( sendedt );
}

//Functions
function sendMsg(){
//Broadcast our Datagram (UDP) packet.
var msg = useredt.GetText() + ": " +  sendedt.GetText();
var packet = mac + "|" + msg;
net.SendDatagram( packet, "UTF-8", address, port );
}

function CheckForMsg(){
    //Try to read a packet for 1 millisec.
	var packet = net.ReceiveDatagram( "UTF-8", port, 1 );
	if( packet ){ 
	    //Extract original parts.
	    parts = packet.split("|");
	    var id = parts[0];
	    var msg = parts[1]; 
	   
	    //Show the message if not sent by this device.
	    if( id != mac )
		    app.ShowPopup( msg);
		    }
		  }
