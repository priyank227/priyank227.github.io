<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <title>Registration Form</title>
    <style>
        input:focus,
        textarea:focus,
        select:focus {
            outline: none;
        }

        input:invalid:focus {
            border: 1px solid red;
        }

        input:valid:focus {
            border: 1px solid green;
        }
    </style>
</head>

<body>
    <div class="relative flex min-h-screen flex-col justify-center overflow-hidden py-6 bg-gray-50">
        <div
            class="relative bg-white px-6 pt-5 pb-5 shadow-xl ring-1 ring-gray-900/5 sm:mx-auto sm:max-w-lg sm:rounded-lg sm:px-10">
            <div class="mx-auto max-w-md">
                <h2 class="text-3xl text-center font-bold leading-tight">
                    Registration Form
                </h2>

                <div class="divide-y divide-gray-300/50">
                    <div class="space-y-6 py-8 text-base leading-7 text-gray-600 text-center">
                        <form id="user-form">
                            <label for="name" class="text-md w-40 text-left inline-block font-medium leading-5 text-gray-700">
                                Name
                            </label>
                            <input required types="text" id="name" name="name"
                                class="bg-gray-100 inline-block rounded-lg shadow-ad px-4 py-3 mb-5 text-base leading-6 placeholder-gray-500 focus:invalid:border-red-500 focus:invalid:ring-red-500 w-52 shadow-md"
                                placeholder="Enter Full Name">
                            </br>

                            <label for="email" class="text-md w-40 text-left inline-block font-medium leading-5 text-gray-700">
                                Email
                            </label>
                            <input required type="email" id="email" name="email"
                                class="bg-gray-100 inline-block rounded-lg shadow-ad px-4 py-3 mb-5 text-base leading-6 placeholder-gray-500 focus:invalid:border-red-500 focus:invalid:ring-red-500 w-52 shadow-md"
                                placeholder="Enter Email">
                            </br>

                            <label for="password" class="text-md w-40 text-left inline-block font-medium leading-5 text-gray-700">
                                Password
                            </label>
                            <input required type="password" id="password" name="password"
                                class="bg-gray-100 inline-block rounded-lg shadow-ad px-4 py-3 mb-5 text-base leading-6 placeholder-gray-500 focus:invalid:border-red-500 focus:invalid:ring-red-500 w-52 shadow-md"
                                placeholder="Enter Password">
                            </br>

                            <label for="dob" class="text-md w-40 text-left inline-block font-medium leading-5 text-gray-700">
                                Date of Birth
                            </label>
                            <input required type="date" id="dob" name="dob"
                                class="bg-gray-100 inline-block rounded-lg shadow-ad px-4 py-3 mb-5 text-base leading-6 placeholder-gray-500 focus:invalid:border-red-500 focus:invalid:ring-red-500 w-52 shadow-md">
                            </br>

                            <div class="w-full text-left">
                                <input required type="checkbox" id="acceptTerms" name="acceptTerms"
                                    class="bg-gray-100 inline-block rounded-lg shadow-ad px-4 py-3 mb-5 text-base leading-6 placeholder-gray-500">
                                <label for="acceptTerms" class="text-md inline-block font-medium leading-5 text-gray-700">
                                    Accept terms & Conditions
                                </label>
                            </div>
                            <br>

                            <input type="submit" id="submit" name="submit" value="Submit"
                                class="w-fit rounded-lg shadow-lg px-8 py-4 bg-green-500 text-white hover:bg-green-400 focus:outline-none focus:shadow-outline transition duration-2">
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <div
            class="relative bg-white px-5 mt-5 pt-5 pb-5 shadow-xl ring-1 ring-gray-900/5 sm:mx-auto sm:rounded-lg sm:px-10">
            <div class="mx-auto">
                <h2 class="text-3xl text-center font-bold leading-tight">
                    Entries
                </h2>

                <div class="divide-y divide-gray-300/50" id="user-entries">
                </div>

            </div>
        </div>
    </div>

    <!-- script -->
    <script>
        // localStorage.clear();
        let date_picker = document.getElementById('dob');
        let now = new Date();
        let day = now.getDate().toString().padStart(2, '0');
        let month = (now.getMonth() + 1).toString().padStart(2, '0');
        let year = now.getFullYear();

        date_picker.min = (year - 55) + "-" + month + "-" + day;
        date_picker.max = (year - 18) + "-" + month + "-" + day;

        const retriveEntries = () => {
            let entries = localStorage.getItem("user-entries");

            if (entries) {
                entries = JSON.parse(entries);
            }
            else {
                entries = [];
            }

            return entries;
        }

        let userForm = document.getElementById("user-form");
        let userEntries = retriveEntries();

        const displayEntries = () => {
            const entries = retriveEntries();
            const tableEntries = entries.map(entry => {
                const nameCell = `<td class='border px-4 py-2'>${entry.name}</td>`;
                const emailCell = `<td class='border px-4 py-2'>${entry.email}</td>`;
                const passwordCell = `<td class='border px-4 py-2'>${entry.password}</td>`;
                const dobCell = `<td class='border px-4 py-2'>${entry.dob}</td>`;
                const acceptTermsCell = `<td class='border px-4 py-2'>${entry.acceptedTermsAndconditions}</td>`;

                const row = `<tr> ${nameCell} ${emailCell} ${passwordCell} ${dobCell} ${acceptTermsCell} </tr>`;
                return row;
            }).join('\n');

            const table = `
                <table class="table-auto w-full">
                    <tr>
                        <th class="px-4 py-2">Name</th>
                        <th class="px-4 py-2">Email</th>
                        <th class="px-4 py-2">Password</th>
                        <th class="px-4 py-2">Dob</th>
                        <th class="px-4 py-2">Accepted terms?</th>
                    </tr>
                    ${tableEntries}
                </table>`;

            let details = document.getElementById('user-entries');
            details.innerHTML = table;

        }

        const saveUserForm = (event) => {
            event.preventDefault();
            const name = document.getElementById("name").value;
            const email = document.getElementById("email").value;
            const password = document.getElementById("password").value;
            const dob = document.getElementById("dob").value;
            const acceptedTermsAndconditions = document.getElementById("acceptTerms").checked;

            const entry = {
                name,
                email,
                password,
                dob,
                acceptedTermsAndconditions
            };

            userEntries.push(entry);

            localStorage.setItem("user-entries", JSON.stringify(userEntries));

            displayEntries();
        }

        userForm.addEventListener("submit", saveUserForm);

        displayEntries();
    </script>
</body>

</html>
