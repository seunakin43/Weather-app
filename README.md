# Weather-app
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() => runApp(MaterialApp(
      title: "Weather App",
      home: Home(),
    ));

class Home extends StatefulWidget {
  // This widget is the root of your application.
  @override
  State<StatefulWidget> createState() {
    return _HomeState();
  }
}

class _HomeState extends State<Home> {
  var temp;
  var description;
  var currently;
  var humidity;
  var lightspeed;
  var precipitation;
  var pressure;
  var coordinate;

  Future getWeather() async {
    http.Response response = await http.get(Uri.parse("https://api.openweathermap.org/data/2.5/weather?q=Lagos,234-1,234&units=imperial&appid=bafcf7ca6f296e845f65b9af73d5117e"));
    var results = jsonDecode(response.body);
    setState(() {
      this.temp = results['main']['temp'];
      this.description = results['weather'][0]['description'];
      this.currently = results['weather'][0]['main'];
      this.humidity = results['main']['humidity'];
      this.lightspeed = results['wind']['speed'];
      this.precipitation = results['clouds']['all'];
      this.pressure = results['main']['pressure'];
      this.coordinate = results['coord']['lon']['lat'];
    });
  }

  @override
  void initState() {
    super.initState();
    this.getWeather();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: <Widget>[
          Container(
            height: MediaQuery.of(context).size.height / 4,
            width: MediaQuery.of(context).size.width,
            color: Colors.blue,
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              crossAxisAlignment: CrossAxisAlignment.center,
              children: <Widget>[
                Padding(
                  padding: EdgeInsets.only(bottom: 12.0),
                  child: Text(
                    "Currently In Lagos",
                    style: TextStyle(color: Colors.black, fontSize: 20.0, fontWeight: FontWeight.w600),
                  ),
                ),
                Text(
                  temp != null ? temp.toString() + "\u00B0\c" : "Loading",
                  style: TextStyle(color: Colors.black, fontSize: 38.0, fontWeight: FontWeight.w600),
                ),
                Padding(
                  padding: EdgeInsets.only(top: 12.0),
                  child: Text(
                    currently != null ? currently.toString() : "Loading",
                    style: TextStyle(color: Colors.tealAccent, fontSize: 18.0, fontWeight: FontWeight.w600),
                  ),
                ),
              ],
            ),
          ),
          Expanded(
            child: Padding(
              padding: EdgeInsets.all(20.0),
              child: ListView(
                children: <Widget>[
                  ListTile(
                    leading: FaIcon(FontAwesomeIcons.thermometerHalf),
                    title: Text("Temperature"),
                    trailing: Text(temp != null ? temp.toString() + "\u00B0\c" : "Loading"),
                  ),
                  ListTile(
                    leading: FaIcon(FontAwesomeIcons.sun),
                    title: Text("Humidity"),
                    trailing: Text(humidity != null ? humidity.toString() + "\u2030" : "Loading"),
                  ),
                  ListTile(
                    leading: FaIcon(FontAwesomeIcons.wind),
                    title: Text("LightSpeed"),
                    trailing: Text(lightspeed != null ? lightspeed.toString() + "km/h" : "loadin-din-din"),
                  ),
                  ListTile(
                    leading: FaIcon(FontAwesomeIcons.cloud),
                    title: Text("Precipitation"),
                    trailing: Text(precipitation != null ? precipitation.toString() + "mm volume" : "Loading"),
                  ),
                  ListTile(
                    leading: FaIcon(FontAwesomeIcons.wind),
                    title: Text("Weather"),
                    trailing: Text(description != null ? description.toString() : "Loading"),
                  ),
                  ListTile(
                    leading: FaIcon(FontAwesomeIcons.cloud),
                    title: Text("Pressure"),
                    trailing: Text(pressure != null ? pressure.toString() + "mmHg" : "Loading"),
                  ),
                  ListTile(
                    leading: FaIcon(FontAwesomeIcons.cloud),
                    title: Text("Curently"),
                    trailing: Text(currently != null ? currently.toString() : "Loading"),
                  ),
                  ListTile(
                    leading: FaIcon(FontAwesomeIcons.compass),
                    title: Text("coordinate"),
                    trailing: Text(coordinate != null ? coordinate.toString() : "Loading"),
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }
}
