# WPS-assignment
let calendarshow = 1;

function settingDate(date, day) {
    date = new Date(date);
    date.setDate(day);//Set the day of the month
    date.setHours(23);
    return date;

}


function getDatesBetween(date1, date2) {
    let range1 = new Date(date1);
    let range2 = new Date(date2);

    console.log(range1 + " " + range2);

    date1 = settingDate(date1, 31);
    date2 = settingDate(date2, 31);
    //console.log(date1 + " " + date2);
    let temp;
    let dates = []
    while (date1 <= date2) {
        if (date1.getDate() != 31) {
            temp = settingDate(date1, 0);
            if (temp >= range1 && temp <= range2) dates.push(temp);
            date1 = settingDate(date1, 31);//It will go to previous month
        }
        else {
            temp = new Date(date1);
            if (temp >= range1 && temp <= range2) dates.push(temp);
            date1.setMonth(date1.getMonth() + 1);//It will go  to starting date of next month
        }
    }
    console.log(dates);//We will get the last date of every month

    let content = "<div class='calendarbtns'> <button id='calendarprev' onclick='callprev()' disabled >Prev</button> | <button id='calendarnext' onclick='callnext()'> Next</button> </div>";
    let weekdays = [
        { shortDay: "Mon", fullday: "Monday" },
        { shortDay: "Tue", fullday: "Tuesday" },
        { shortDay: "Wed", fullday: "Wednesday" },
        { shortDay: "Thu", fullday: "Thursday" },
        { shortDay: "Fri", fullday: "Friday" },
        { shortDay: "Sat", fullday: "Saturday" },
        { shortDay: "Sun", fullday: "Sunday" }
    ];


    let lastdate, firstdate;
    for (let i = 0; i < dates.length; i++) {
        lastdate = dates[i];
        firstdate = new Date(dates[i].getFullYear(), dates[i].getMonth(), 1);
        content += "<div id='calendarTable_" + (i + 1) + "' class='calendardiv'>";
        content += "<h2>" + firstdate.toString().split(" ")[1] + "-" + firstdate.getFullYear() + "</h2>";//Fri jan 31 2020 23:00:00 GMT+0530 here it will give the jan 2020

        content += "<table class='calendartable' >";
        content += "<thead>";

        weekdays.map(item => {
            content += "<th>" + item.fullday + "</th>";
        });

        content += "</thead>";

        content += "<tbody>";
        let j = 1;
        let displaynum;
        while (j <= lastdate.getDate()) {
            content += "<tr>";

            for (let k = 0; k < 7; k++) {

                displaynum = j < 10 ? "0" + j : j;
                console.log(displaynum);
                if (j == 1) {
                    if (firstdate.toString().split(" ")[0] == weekdays[k].shortDay) {

                        content += " <td id='" + (i + 1) + "_ask" + (displaynum) + "' onclick='askevent( " + displaynum + "," + (i + 1) + " )'> <h3>" + displaynum + "</h3></td>";
                        j++;
                    }
                    else {
                        content += "<td></td>";
                    }
                }
                else if (j > lastdate.getDate()) {
                    content += "<td></td>";
                }
                else {
                    content += "<td id='" + (i + 1) + "_ask" + (displaynum) + "' onclick='askevent(" + displaynum + "," + (i + 1) + ")'> <h3>" + displaynum + "</h3></td>";
                    j++;
                }
            }

            content += "</tr>";
        }

        content += "</tbody>";
        content += "</table>";

        content += "</div>";


    }

    return content;

}

//If we will give 2020/04/01 i.e April 1 and pass 31 as setting date it will return 
//May1, since april does not have 31 days and if we pass 0 as parameter from setting date 
//it will return march31 and if we give 32 then May 2 setDate will take 1-31 anything above
//that will go to next month and anything below will go to previous month

function callnext() {
    let alltable = document.getElementsByClassName("calendardiv");
    document.getElementById('calendarprev').disabled = false;
    calendarshow++;
    if (calendarshow <= alltable.length) {
        for (let i = 0; i < alltable.length; i++) {
            alltable[i].style.display = "none";
        }
        document.getElementById("calendarTable_" + calendarshow).style.display = "block";
        if (calendarshow == alltable.length) {
            document.getElementById("calendarnext").disabled = true;
        }

    }
}

function callprev() {
    let alltable = document.getElementsByClassName("calendardiv");
    document.getElementById('calendarnext').disabled = false;
    calendarshow--;
    if (calendarshow >= 1) {
        for (let i = 0; i < alltable.length; i++) {
            alltable[i].style.display = "none";
        }
        document.getElementById("calendarTable_" + calendarshow).style.display = "block";
        if (calendarshow == 1) {
            document.getElementById("calendarprev").disabled = true;
        }

    }
}

function askevent(num, id) {

    var c = prompt("Please Enter Event name", "Event");
    num = num < 10 ? "0" + num : num;
    console.log(num);
    if (c != null) {
        document.getElementById("" + id + "_ask" + num).innerHTML = "<h2>" + num + "</h2> <h2 style='color : red'>" + c + "</h2>";

    }

}



let content = getDatesBetween("2020/01/01", "2021/01/01");

document.getElementById("calendar").innerHTML = content;
