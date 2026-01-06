# Clinic-Appointment-Booking-System
//Clinic Appointment Booking System
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <limits>

using namespace std;

const int MAX_APPTS = 500;
const string DATA_FILE = "appoinments.txt";

struct Appointment {
	int id ;			
	string patientName ;
	string phone ;
	string date ;   // YYYY-MM-DD
	string time ;   // HH:MM
	string doctor ;
	string notes ;
	bool active ;
};

// === Global Data ===
Appointment appts[MAX_APPTS];
int apptCount = 0;
int nextID = 1;

//utility; trim helpers
string trim(const string& s){
size_t start =
s.find_first_not_of("\t\r\n");
if (start == string::npos)return "";
size_t end = s.find_last_not_of("\t\r\n");
return s.substr(start, end - start + 1);
}

// load appointments from file (chelsea)

//Load all data from files
void loadFromFile() {
	//load appointments
	ifstream apptFile(APPOINTMENT_FILE);
	if (apptFile) {
		string line;
		spptCount = 0;
		while (getline(apptFile, line) && apptCount < MAX_APPTS) {
			if (trim(linr).empty()) continue;

			stringstream ss(line);
			string token;
			Appointment a;

            getline(ss, token, '|'); a.id = stoi(token);
            getline(ss, a.patientName, '|');
            getline(ss, a.phone, '|');
            getline(ss, a.date, '|');
            getline(ss, a.time, '|');
            getline(ss, a.doctor, '|');
            getline(ss, a.specialization, '|');
            getline(ss, a.notes, '|');
            getline(ss, a.status, '|');

            appointments[apptCount++] = a;
            if (a.id >= nextApptID) nextApptID = a.id + 1;
        }
        apptFile.close();
    }

    // Load patients
    ifstream patientFile(PATIENT_FILE);
    if (patientFile) {
        string line;
        patientCount = 0;
        while (getline(patientFile, line) && patientCount < MAX_PATIENTS) {
            if (trim(line).empty()) continue;

            stringstream ss(line);
            string token;
            Patient p;

            getline(ss, token, '|'); p.id = stoi(token);
            getline(ss, p.name, '|');
            getline(ss, token, '|'); p.age = stoi(token);
            getline(ss, p.phone, '|');
            getline(ss, p.email, '|');
            getline(ss, p.medicalHistory, '|');

            patients[patientCount++] = p;
            if (p.id >= nextPatientID) nextPatientID = p.id + 1;
        }
        patientFile.close();
    }

    // Load doctors
    ifstream doctorFile(DOCTOR_FILE);
    if (doctorFile) {
        string line;
        doctorCount = 0;
        while (getline(doctorFile, line) && doctorCount < MAX_DOCTORS) {
            if (trim(line).empty()) continue;

            stringstream ss(line);
            string token;
            Doctor d;

            getline(ss, token, '|'); d.id = stoi(token);
            getline(ss, d.name, '|');
            getline(ss, d.specialization, '|');
            getline(ss, d.availableDays, '|');
            getline(ss, d.availableTime, '|');
            getline(ss, token, '|'); d.maxPatients = stoi(token);

            doctors[doctorCount++] = d;
            if (d.id >= nextDoctorID) nextDoctorID = d.id + 1;
        }
        doctorFile.close();
    }

    cout << "Loaded " << apptCount << " appointments, "
        << patientCount << " patients, "
        << doctorCount << " doctors.\n";
}


//Ung Pun Kang
// Save appointments to file
void saveToFile()
{
    ofstream outfile("DATA_FILE");

    if (!outfile)
    {
        cout << "Error opening file.\n";
        return;
    }

    for (int i = 0; i < apptCount; i++)
    {
        outfile << appts[i].id << "|"
            << appts[i].patientName << "|"
            << appts[i].phone << "|"
            << appts[i].date << "|"
            << appts[i].time << "|"
            << appts[i].doctor << "|"
            << appts[i].notes << "|";

        if (appts[i].active)
            outfile << "1\n";
        else
            outfile << "0\n";
    }

    outfile.close();
    return 0;
}

//Nuhaa
// Display one appointment
void displayAppointment(const Appointment& a) {
    cout << "ID: " << a.id << (a.active ? " (Active)\n" : " (Cancelled)\n");
    cout << " Patient: " << a.patientName << "\n";
    cout << " Phone:   " << a.phone << "\n";
    cout << " Date:    " << a.date << "\n";
    cout << " Time:    " << a.time << "\n";
    cout << " Doctor:  " << a.doctor << "\n";
    cout << " Notes:   " << a.notes << "\n";
    cout << "-----------------------------\n";
}

//Nuhaa
// List all appointments
void listAppointments(bool showAll = true) {
    if (apptCount == 0) {
        cout << "No appointments.\n";
        return;
    }
    for (int i = 0; i < apptCount; ++i) {
        if (!showAll && !appts[i].active) continue;
        displayAppointment(appts[i]);
    }
}

// Yixin
// Add appointment
void addAppointment() {
    if (apptCount >= MAX_APPTS) {
        cout << "Appointment list is full.\n";
        return;
    }

    Appointment newAppt;
    newAppt.id = nextID++;
    newAppt.active = true;

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    cout << "Enter patient name: ";
    getline(cin, newAppt.patientName);

    cout << "Enter phone number: ";
    getline(cin, newAppt.phone);

    cout << "Enter appointment date (YYYY-MM-DD): ";
    getline(cin, newAppt.date);

    cout << "Enter appointment time (HH:MM): ";
    getline(cin, newAppt.time);

    cout << "Enter doctor name: ";
    getline(cin, newAppt.doctor);

    cout << "Enter notes (optional): ";
    getline(cin, newAppt.notes);

    appts[apptCount++] = newAppt;

    cout << "Appointment added successfully. ID: "
        << newAppt.id << endl;
}
//find index by id (chelsea)

int findAppointmentIndexByID(int id) {
    for (int i = 0; i < apptCount; i++) {
        if (appointments[i].id == id) {
            return i;  // Return array index where found
        }
    }
    return -1;  // Return -1 if not found
}

// Find patient index by ID (returns -1 if not found)
int findPatientIndexByID(int id) {
    for (int i = 0; i < patientCount; i++) {
        if (patients[i].id == id) {
            return i;
        }
    }
    return -1;
}

// Find doctor index by ID (returns -1 if not found)
int findDoctorIndexByID(int id) {
    for (int i = 0; i < doctorCount; i++) {
        if (doctors[i].id == id) {
            return i;
        }
    }
    return -1;
}

// Find doctor index by name (returns -1 if not found)
int findDoctorIndexByName(const string& doctorName) {
    for (int i = 0; i < doctorCount; i++) {
        if (doctors[i].name == doctorName) {
            return i;
        }
    }
    return -1;
}

// Find patient index by name (returns -1 if not found, first match)
int findPatientIndexByName(const string& patientName) {
    for (int i = 0; i < patientCount; i++) {
        if (patients[i].name == patientName) {
            return i;
        }
    }
    return -1;
}

// Find appointment index by patient ID and date (for duplicate checking)
int findAppointmentIndexByPatientAndDate(int patientId, const string& date) {
    for (int i = 0; i < apptCount; i++) {
        if (appointments[i].patientName == to_string(patientId) &&
            appointments[i].date == date) {
            return i;
        }
    }
    return -1;
}

// Find all appointment indices by patient ID (returns vector of indices)
vector<int> findAllAppointmentIndicesByPatient(int patientId) {
    vector<int> indices;
    for (int i = 0; i < apptCount; i++) {
        if (stoi(appointments[i].patientName) == patientId) {
            indices.push_back(i); 
        }
    }
    return indices;
}

// Find all appointment indices by doctor name (returns vector of indices)
vector<int> findAllAppointmentIndicesByDoctor(const string& doctorName) {
    vector<int> indices;
    for (int i = 0; i < apptCount; i++) {
        if (appointments[i].doctor == doctorName) {
            indices.push_back(i);
        }
    }
    return indices;
}

// Find appointment index by date and time (for time slot checking)
int findAppointmentIndexByDateTime(const string& date, const string& time, const string& doctor) {
    for (int i = 0; i < apptCount; i++) {
        if (appointments[i].date == date &&
            appointments[i].time == time &&
            appointments[i].doctor == doctor &&
            appointments[i].status == "Scheduled") {
            return i;
        }
    }
    return -1;
}

// Find all appointment indices by doctor name (returns vector of indices)
vector<int> findAllAppointmentIndicesByDoctor(const string& doctorName) {
    vector<int> indices;
    for (int i = 0; i < apptCount; i++) {
        if (appointments[i].doctor == doctorName) {
            indices.push_back(i);
        }
    }
    return indices;
}

// Find appointment index by date and time (for time slot checking)
int findAppointmentIndexByDateTime(const string& date, const string& time, const string& doctor) {
    for (int i = 0; i < apptCount; i++) {
        if (appointments[i].date == date &&
            appointments[i].time == time &&
            appointments[i].doctor == doctor &&
            appointments[i].status == "Scheduled") {
            return i;
        }
    }
    return -1;
}

// Yixin
// Search appointment by Patient Name
// Search Appointment by Patient Name
void searchAppointmentByName() {
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    string keyword;
    cout << "Enter patient name to search: ";
    getline(cin, keyword);

    bool found = false;

    for (int i = 0; i < apptCount; i++) {
        if (appts[i].patientName.find(keyword) != string::npos) {
            displayAppointment(appts[i]);
            found = true;
        }
    }

    if (!found) {
        cout << "No appointment found.\n";
    }
}

//edit appointment (chelsea)

// Edit appointment
void editAppointment() {
    cout << "Enter appointment ID to edit: ";
    int id;
    if (!(cin >> id)) {
        clearInput();
        cout << "Invalid ID!\n";
        return;
    }
    
    int idx = findAppointmentByID(id);
    if (idx == -1) {
        cout << "Appointment not found!\n";
        return;
    }
    
    Appointment &a = appointments[idx];
    clearInput();
    
    cout << "\nEditing appointment ID " << a.id << "\n";
    cout << "Leave field blank to keep current value.\n";
    
    string input;
    
    cout << "Current patient: " << a.patientName << "\nNew patient: ";
    getline(cin, input);
    if (!trim(input).empty()) a.patientName = input;
    
    cout << "Current phone: " << a.phone << "\nNew phone: ";
    getline(cin, input);
    if (!trim(input).empty()) a.phone = input;
    
    cout << "Current date: " << a.date << "\nNew date (YYYY-MM-DD): ";
    getline(cin, input);
    if (!trim(input).empty() && isValidDate(input)) a.date = input;
    
    cout << "Current time: " << a.time << "\nNew time (HH:MM): ";
    getline(cin, input);
    if (!trim(input).empty() && isValidTime(input)) a.time = input;
    
    cout << "Current doctor: " << a.doctor << "\nNew doctor: ";
    getline(cin, input);
    if (!trim(input).empty()) a.doctor = input;
    
    cout << "Current notes: " << a.notes << "\nNew notes: ";
    getline(cin, input);
    if (!trim(input).empty()) a.notes = input;
    
    cout << "Current status: " << a.status << "\nNew status (Scheduled/Completed/Cancelled): ";
    getline(cin, input);
    if (!trim(input).empty()) a.status = input;
    
    cout << "Appointment updated successfully!\n";
}


// Yixin
// Cancel Appointment
void cancelAppointment() {
    int id;
    cout << "Enter appointment ID to cancel: ";
    cin >> id;

    bool found = false;

    for (int i = 0; i < apptCount; i++) {
        if (appts[i].id == id) {
            appts[i].active = false;
            cout << "Appointment cancelled successfully.\n";
            found = true;
            break;
        }
    }

    if (!found) {
        cout << "Appointment ID not found.\n";
    }
}

//Amalina
// Delete appointment (physically remove from array)
void deleteAppointment() {
    cout << "Enter appointment ID to delete permanently: ";
    int id; if (!(cin >> id)) { cin.clear(); cin.ignore(numeric_limits<streamsize>::max(), '\n'); cout << "Invalid input.\n"; return; }
    int idx = findByID(id);
    if (idx == -1) { cout << "Not found.\n"; return; }
    // shift left
    for (int i = idx; i < apptCount - 1; ++i) appts[i] = appts[i + 1];
    --apptCount;
    cout << "Appointment deleted.\n";
}

//Nuhaa
// Simple menu
void showMenu() {
    cout << "\n=== Clinic Appointment System ===\n";
    cout << "1. Add appointment\n";
    cout << "2. List all appointments\n";
    cout << "3. Search by patient name\n";
    cout << "4. Edit appointment\n";
    cout << "5. Cancel appointment (mark inactive)\n";
    cout << "6. Delete appointment (permanent)\n";
    cout << "7. Save to file\n";
    cout << "8. Load from file\n";
    cout << "9. List only active appointments\n";
    cout << "0. Exit\n";
    cout << "Select option: ";
}

//Amalina
int main() {
    loadFromFile();
    int choice;
    do {
        showMenu();
        if (!(cin >> choice)) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid option. Try again.\n";
            continue;
        }
        switch (choice) {
        case 1: addAppointment(); break;
        case 2: listAppointments(true); break;
        case 3: {
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Enter name to search: ";
            string q; getline(cin, q);
            searchByName(q);
            break;
        }
        case 4: editAppointment(); break;
        case 5: cancelAppointment(); break;
        case 6: deleteAppointment(); break;
        case 7: saveToFile(); break;
        case 8: loadFromFile(); break;
        case 9: listAppointments(false); break;
        case 0: cout << "Exiting. Goodbye.\n"; break;
        default: cout << "Unknown option.\n"; break;
        }
    } while (choice != 0);

    // optional: auto-save on exit
    saveToFile();
    return 0;
}
