# SIMLAB-DOCKERPYTHON - SOURCE CODE

import urllib.parse
import requests

main_api = "https://www.mapquestapi.com/directions/v2/route?"
key = "xDRzvNdSksgbOzoRqN59TI2kjufJWgAE"


while True:
    startPos = input("Uw startlocatie(press q to quit): ")
    if startPos == "stop" or startPos == "s" or startPos == "q" or startPos == "quit":
        break
    endPos = input("Uw bestemming(press q to quit): ")
    if endPos == "stop" or endPos == "s" or endPos == "q" or endPos == "quit":
        break
    #bloat the script by adding more ways to end it prematurely!
    url = main_api + urllib.parse.urlencode({"key": key, "from":startPos, "to":endPos})
    print("Request verstuurd naar: " + (url))

    json_data = requests.get(url).json()
    json_status = json_data["info"]["statuscode"]

    if json_status == 0:
        print("API Status: " + str(json_status))
        print("+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+")
        print("Route van " + (startPos) + " naar " + (endPos))
        print("Duratie:   " + (json_data["route"]["formattedTime"]))
        print("Afstand in kilometer: " + str(str(json_data["route"]["distance"]*1.61)))
        print("Benzine verbruikt in liter: " + str("{:.2f}".format((json_data["route"]["fuelUsed"])*3.78)))
        print("+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+")
    elif json_status == 402:
        print("**********************************************")
        print("Status Code: " + str(json_status) + "; Invalid user inputs for one or both locations.")
        print("**********************************************\n")
    elif json_status == 404:
        print("**********************************************")
        print("Shouldn't be happening but ok")
        print("**********************************************\n")
        #bloat the script even more!
    else:
        print("************************************************************************")
        print("Yeah dude, no idea what the issue is. HTTP error: " + str(json_status) + "; go to:")
        print("https://developer.mapquest.com/documentation/directions-api/status-codes")
        print("Or spam Koen on Webex Teams")
        print("************************************************************************\n")


    showExactRoute = False
    yesOrNo = input("Do you want to see the exact route? Y/N: ")
    if yesOrNo == "y" or yesOrNo == "Y":
        showExactRoute = True
    else:
        print("Ok bye")
        break
    if showExactRoute == True:
        print("=============================================")
        for each in json_data["route"]["legs"][0]["maneuvers"]:
            print((each["narrative"]) + " (" + str("{:.2f}".format((each["distance"])*1.61) + " km)"))
        print("=============================================\n")


