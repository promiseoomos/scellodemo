<template>
    <div>
        <div class="bg-gray-200 p-5 font-sans">
            <p class="text-2xl font-bold">Users Payment Status</p>
            <div class="flex flex-wrap justify-between mt-3 border-b border-gray-400/[0.6] ">
                <div class="flex flex-wrap gap-4 justify-start px-2">
                    <button @click="showing = 'All'" class="w-fit w-min-[10]">All <span :class="{'border-b-2 border-blue-800' : showing == 'All'}" class="relative top-2 w-full inline-block"></span></button>
                    <button @click="showing = 'Paid'" class="w-fit">Paid <span :class="{'border-b-2 border-blue-800' : showing == 'Paid'}" class="relative top-2 w-full inline-block"></span></button>
                    <button @click="showing = 'Unpaid'" class="w-fit">Unpaid <span :class="{'border-b-2 border-blue-800' : showing == 'Unpaid'}" class="relative  top-2 w-full inline-block"></span></button>
                    <button @click="showing = 'Overdue'" class="w-fit">Overdue <span :class="{'border-b-2 border-blue-800' : showing == 'Overdue'}" class="relative  top-2 w-full inline-block"></span></button>
                </div>
                <div class="">
                    <p class=" text-[#6E6893]">Total Payable amount: <span class="text-3xl font-bold text-[#6D5BD0]">$900</span><span class="text-3xl text-[#6E6893]"> USD</span></p>
                </div>
            </div>

            <div class="rounded-lg bg-white border border-gray-100 mt-7">

                <div class="flex flex-wrap justify-between p-3">
                    <div class="w-1/2">
                        <button class="border border-[#C6C2DE] p-3 text-xl mr-4 rounded-lg w-24 text-[#25213B]"><img src="~/assets/filter.png" class="inline-block"/> Filter</button>
                        <input @keyup="searcher()" v-model="searchtext" class="p-3 pl-12 w-4/5 bg-gray-100 outline-0 focus:outline-0 hover:border hover:border-[#6D5BD0] rounded-lg" placeholder="Search User by Name, Email or Date">
                        <img src="~/assets/search.png" class="relative -top-8 left-32 z-50">
                    </div>
                    <div class="">
                        <button class="p-3 bg-[#6D5BD0] text-white font-bold rounded-lg">PAY DUES</button>
                    </div>
                </div>

                <div class="flex flex-nowrap bg-[#F4F2FF] p-4 pl-11 border-y-1 border-[#D9D5EC] overflow-x-auto text-[#6E6893]">
                    <div class="w-1/3 overflow-x-auto text-xs"><input type="checkbox" v-model="checkall" class="p-2 w-5 h-5 rounded-md"/></div>
                    <div class="w-1/2 overflow-x-auto text-xs">NAME</div>
                    <div class="w-9/12 overflow-x-auto text-xs">USER STATUS</div>
                    <div class="w-5/12 overflow-x-auto text-xs">PAYMENT STATUS</div>
                    <div class="w-1/4 overflow-x-auto text-xs">AMOUNT</div>
                    <!-- <div class="w-1/3 overflow-x-auto text-xs"></div> -->
                    <div class="w-1/5 overflow-x-auto text-xs text-right"><img src="~/assets/More.png" class="inline-block"></div>
                </div>
                <div class="" v-if="searchtext.length == 0">
                    <div v-for="(userdata, index) in usingsliceddata" :key="index" class="">
                        <div v-if="showing == 'All' || userdata.payment_status == showing" class="flex flex-nowrap p-4 pl-11 pt-3 overflow-x-auto border-b border-[#D9D5EC]" :class="{'bg-[#F2F0F9]' : currentitemid == userdata.trx_id && currentlychecked == true }">
                            <div class="w-1/3 overflow-x-auto text-xs"><input type="checkbox" @change="currentlyOn(userdata.trx_id, $event)" class="p-2 w-5 h-5 rounded-md inline-block"/> <button @click="showingdownid = userdata.trx_id; showingdown = !showingdown" class="inline-block ml-4 mt-3"><img v-if="showingdown == true" src="~/assets/Up.png"  class="-mt-3 ml-4 h-5 w-5 inline-block"><img v-if="showingdown == false" src="~/assets/down-arrow.png"  class="-mt-3 ml-4 h-5 w-5 inline-block"></button></div>
                            <div class="w-1/2 overflow-x-auto text-sm">{{ userdata.first_name }} {{ userdata.last_name }} <br> <span class="text-gray-400 text-xs">{{ userdata.email }}</span></div>
                            <div class="w-9/12 overflow-x-auto text-xs">
                                <span class="rounded-2xl px-2" :class="{ 'bg-[#E6E6F2] text-[#4A4AFF]' : userdata.user_status == 'Active', 'text-[#6E6893] bg-[#F2F0F9]' : userdata.user_status == 'Inactive'}"><span :class="{ 'bg-[#4A4AFF]' : userdata.user_status == 'Active', 'bg-[#6E6893]' : userdata.user_status == 'Inactive'}" class="w-2 h-2 mr-1 inline-block rounded-full"></span>{{ userdata.user_status }}</span>
                                <p class="py-2 text-gray-400">Last login : {{ userdata.last_seen }}</p>
                            </div>
                            <div class="w-5/12 overflow-x-auto text-xs">
                                <span class="rounded-2xl px-2" :class="{ 'bg-[#CDFFCD] text-[#007F00]' : userdata.payment_status == 'Paid', 'text-[#CE8500] bg-[#FFECCC]' : userdata.payment_status == 'Unpaid', 'text-[#D30000] bg-[#FFE0E0]' : userdata.payment_status == 'Overdue'}"><span :class="{ 'bg-[#007F00]' : userdata.payment_status == 'Paid', 'bg-[#CE8500]' : userdata.payment_status == 'Unpaid', 'bg-[#D30000]' : userdata.payment_status == 'Overdue'}" class="w-2 h-2 mr-1 inline-block rounded-full"></span>{{ userdata.payment_status }}</span>
                                <p class="py-2 text-gray-400">{{ userdata.payment_status == 'Paid' ? 'Paid on' : userdata.payment_status == 'Unpaid' ? 'Dues on' : userdata.payment_status == 'Overdue' ? 'Dued on' : '' }} {{ userdata.payment_status == 'Paid' ? userdata.payment_date : userdata.due_date}}</p>
                            </div>
                            <div class="w-1/4 overflow-x-auto text-md">{{ userdata.symbol }}{{ userdata.amount }} <br><span class="block pl-2 text-sm text-gray-400">{{ userdata.currency }}</span></div>
                            <!-- <div class="w-1/3 overflow-x-auto text-xs">view more</div> -->
                            <div class="w-1/5 overflow-x-auto text-xs text-right">
                                <button><span class="inline-block mr-3 text-[#6E6893]">View more</span> <img src="~/assets/More.png" class="inline-block"></button>
                            
                            </div>
                            
                        </div>
                        <div class="absolute hidden h-contain m-2 p-2 z-5 w-40 right-2 bg-gray-200">
                            
                            <div class="absolute -top-4 right-2 h-fit bg-white"><img src="~/assets/ellipse.png"  class=""> <span class="relative -right-2 -top-6">x</span></div>
                            <NuxtLink class="block">Edit</NuxtLink>
                            <NuxtLink class="block">View Profile</NuxtLink>
                            <NuxtLink class="block text-[#007F00]">Activate User</NuxtLink>
                            <NuxtLink class="block text-[#D30000] mt-2">Delete</NuxtLink>
                        </div>

                        <div class="bg-[#F4F2FF]" v-if="showingdownid == userdata.trx_id && showingdown == true">
                            <div class="flex flex-nowrap bg-[#F2F0F9] p-4 pl-40 border-y-1 border-[#D9D5EC] overflow-x-auto">
                                <div class="w-1/4 overflow-x-auto text-xs">DATE</div>
                                <div class="w-1/4 overflow-x-auto text-xs">USER ACTIVITY</div>
                                <div class="w-1/4 overflow-x-auto text-xs">DETAILS <img src="~/assets/details.png" class="inline-block ml-3 -mt-1"></div>
                            </div>
                            <div v-if="userdata.user_activity.length > 0">
                                <div v-for="(activity, index) in userdata.user_activity" :key="index" class="">
                                    <div class="flex flex-nowrap p-4 pl-40 mt-3 overflow-x-auto border-b border-[#D9D5EC]">
                                        <div class="w-1/4 overflow-x-auto text-sm">{{ activity.date }}</div>
                                        <div class="w-1/4 overflow-x-auto text-xs">
                                            {{ activity.activity }}
                                        </div>
                                        <div class="w-1/4 overflow-x-auto text-xs">
                                            {{ activity.details }}
                                        </div>
                                    </div>

                                </div>
                            </div>
                            <div v-else>
                                <p class="text-xl text-center p-5 text-[#6E6893]">NO RECORDS FOUND</p>
                            </div>
                        </div>
                        
                    </div>                
                </div>
                <div v-else>
                    <div v-for="(userdata, index) in usingsliceddata" :key="index" class="">
                        <div v-if="showing == 'All' || userdata.payment_status == showing" class="flex flex-nowrap p-4 pl-11 mt-3 overflow-x-auto border-b border-[#D9D5EC]">
                            <div class="w-1/3 overflow-x-auto text-xs"><input type="checkbox" v-model="checkall" class="p-2 w-5 h-5 rounded-md inline-block"/> <button @click="showingdownid = userdata.trx_id; showingdown = !showingdown" class="inline-block ml-4 mt-3"><img src="~/assets/down-arrow.png"  class="-mt-3 ml-4 h-5 w-5 inline-block"></button></div>
                            <div class="w-1/2 overflow-x-auto text-sm">{{ userdata.first_name }} {{ userdata.last_name }} <br> <span class="text-gray-400 text-xs">{{ userdata.email }}</span></div>
                            <div class="w-9/12 overflow-x-auto text-xs">
                                <span class="rounded-2xl px-2" :class="{ 'bg-[#E6E6F2] text-[#4A4AFF]' : userdata.user_status == 'Active', 'text-[#6E6893] bg-[#F2F0F9]' : userdata.user_status == 'Inactive'}"><span :class="{ 'bg-[#4A4AFF]' : userdata.user_status == 'Active', 'bg-[#6E6893]' : userdata.user_status == 'Inactive'}" class="w-2 h-2 mr-1 inline-block rounded-full"></span>{{ userdata.user_status }}</span>
                                <p class="py-2 text-gray-400">Last login : {{ userdata.last_seen }}</p>
                            </div>
                            <div class="w-5/12 overflow-x-auto text-xs">
                                <span class="rounded-2xl px-2" :class="{ 'bg-[#CDFFCD] text-[#007F00]' : userdata.payment_status == 'Paid', 'text-[#CE8500] bg-[#FFECCC]' : userdata.payment_status == 'Unpaid', 'text-[#D30000] bg-[#FFE0E0]' : userdata.payment_status == 'Overdue'}"><span :class="{ 'bg-[#007F00]' : userdata.payment_status == 'Paid', 'bg-[#CE8500]' : userdata.payment_status == 'Unpaid', 'bg-[#D30000]' : userdata.payment_status == 'Overdue'}" class="w-2 h-2 mr-1 inline-block rounded-full"></span>{{ userdata.payment_status }}</span>
                                <p class="py-2 text-gray-400">{{ userdata.payment_status == 'Paid' ? 'Paid on' : userdata.payment_status == 'Unpaid' ? 'Dues on' : userdata.payment_status == 'Overdue' ? 'Dued on' : '' }} {{ userdata.payment_status == 'Paid' ? userdata.payment_date : userdata.due_date}}</p>
                            </div>
                            <div class="w-1/4 overflow-x-auto text-md">{{ userdata.symbol }}{{ userdata.amount }} <br><span class="block pl-2 text-sm text-gray-400">{{ userdata.currency }}</span></div>
                            <!-- <div class="w-1/3 overflow-x-auto text-xs">view more</div> -->
                            <div class="w-1/5 overflow-x-auto text-xs text-right">
                                <button><span class="inline-block mr-3 text-[#6E6893]">View more</span> <img src="~/assets/More.png" class="inline-block"></button>
                            </div>
                        </div>

                        <div class="bg-[#F4F2FF]" v-if="showingdownid == userdata.trx_id && showingdown == true">
                            <div class="flex flex-nowrap bg-[#F2F0F9] p-4 pl-40 border-y-1 border-[#D9D5EC] overflow-x-auto">
                                <div class="w-1/4 overflow-x-auto text-xs">DATE</div>
                                <div class="w-1/4 overflow-x-auto text-xs">USER ACTIVITY</div>
                                <div class="w-1/4 overflow-x-auto text-xs">DETAILS <img src="~/assets/details.png" class="inline-block ml-3 -mt-1"></div>
                            </div>
                            <div v-if="userdata.user_activity.length > 0">
                                <div v-for="(activity, index) in userdata.user_activity" :key="index" class="">
                                    <div class="flex flex-nowrap p-4 pl-40 mt-3 overflow-x-auto border-b border-[#D9D5EC]">
                                        <div class="w-1/4 overflow-x-auto text-sm">{{ activity.date }}</div>
                                        <div class="w-1/4 overflow-x-auto text-xs">
                                            {{ activity.activity }}
                                        </div>
                                        <div class="w-1/4 overflow-x-auto text-xs">
                                            {{ activity.details }}
                                        </div>
                                    </div>

                                </div>
                            </div>
                            <div v-else>
                                <p class="text-xl text-center p-5 text-[#6E6893]">NO RECORDS FOUND</p>
                            </div>
                        </div>
                        
                    </div>
                </div>




                <div v-if="searchtext.length == 0" class="flex flex-nowrap gap-24 justify-end bg-[#F4F2FF] text-[#6E6893] p-6">
                    <div class="">Rows per page 
                        <select v-model="intervalnum" @change="increaserows" class="bg-transparent">
                            <option v-for="(option, index) in perpageoptions" :key="index" :value="option">{{ option }}</option>
                        </select> 
                    
                    </div>
                    <div class="">{{ usingusersdata.length > 0 ?  startnum + 1 : 0 }} - {{ usingusersdata.length < endnum ? usingusersdata.length : endnum }} of {{ usingusersdata.length }}</div>
                    <div class="flex gap-16"><button @click="pages > 1 ? pages = pages - 1 : pages = 1"><img src="~/assets/arrow-left.png" class="h-fit"/></button> <button @click="pages < Math.ceil(usingusersdata.length/ intervalnum) ? pages = pages + 1 : pages = Math.ceil(usingusersdata.length/ intervalnum)"><img src="~/assets/arrow-right.png" class="h-fit"/></button> </div>
                </div>
                <div v-else class="flex flex-nowrap gap-24 justify-end bg-[#F4F2FF] text-[#6E6893] p-6">
                    <div class="">Rows per page 
                        <select v-model="intervalnum" class="bg-transparent">
                            <option v-for="(option, index) in perpageoptions" :key="index" :value="option">{{ option }}</option>
                        </select> 
                    
                    </div>
                    <div class="">{{ searchresult.length > 0 ?  startnum + 1 : 0 }} - {{ searchresult.length < endnum ? searchresult.length : endnum }} of {{ searchresult.length }}</div>
                    <div class="flex gap-16"><button @click="pages > 1 ? pages = pages - 1 : pages = 1"><img src="~/assets/arrow-left.png" class="h-fit"/></button> <button @click="pages < Math.ceil(searchresult.length / intervalnum) ? pages = pages + 1 : pages = Math.ceil(searchresult.length / intervalnum)"><img src="~/assets/arrow-right.png" class="h-fit"/></button> </div>
                </div>
            
            </div>
        </div>

        <!-- <p>{{ usingusersdata.users[0].first_name }}</p> -->
    </div>
</template>

<script setup>
const users = await useFetch("https://cornie-assessment.herokuapp.com/users/CcUyJOFdB0q52GB")
let showing = ref('All')
let showingdown = ref(false)
let showingdownid = ref(0)
// let intervalnum = ref(10)
const perpageoptions = ref([10, 25, 50, 100, 250, 500])
const searchtext = ref("")
let searchresult = ref([])
let checkall = ref(false)

let usingusersdata = ref([])
let usingsliceddata = ref([])

const usersdata = reactive([
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybankz@gmail.com",
            "trx_id" : 10238475,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_status" : "Paid",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : [
                {
                    "date" : "12/02/2021",
                    "activity" : "Made a Purchase Order",
                    "details" : "The User made a Purchase Order for 5 Goods and Payed via Card. The User also Paid for delivery via Courier services"
                },
                {
                    "date" : "10/02/2021",
                    "activity" : "Checked Price of Goods",
                    "details" : "The user navigated to the Pricing page of 'Anti-blue Reflection Glass' and checked the price of the Goods"
                },
                {
                    "date" : "2/02/2021",
                    "activity" : "User cancelled Order",
                    "details" : "User requested Cancellation of Order on 3 goods citing reasons that 'Payment Gateway not responding"
                }
            ]
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10983475,
            "user_status" : "Inactive",
            "last_seen" : "09/20/2021",
            "payment_status" : "Paid",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : [
                {
                    "date" : "12/02/2021",
                    "activity" : "Made a Sales Order",
                    "details" : "The User made a Purchase Order for 5 Goods and Payed via Card. The User also Paid for delivery via Courier services"
                },
                {
                    "date" : "10/02/2021",
                    "activity" : "Checked Price of Services",
                    "details" : "The user navigated to the Pricing page of 'Anti-blue Reflection Glass' and checked the price of the Goods"
                },
                {
                    "date" : "2/02/2021",
                    "activity" : "User cancelled Order",
                    "details" : "User requested Cancellation of Order on 3 goods citing reasons that 'Payment Gateway not responding"
                }
            ]
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10486475,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_date" : "12/12/2021",
            "payment_status" : "Unpaid",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : []
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10232195,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_status" : "Overdue",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : []
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybankz@gmail.com",
            "trx_id" : 10238475,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_status" : "Paid",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : [
                {
                    "date" : "12/02/2021",
                    "activity" : "Made a Purchase Order",
                    "details" : "The User made a Purchase Order for 5 Goods and Payed via Card. The User also Paid for delivery via Courier services"
                },
                {
                    "date" : "10/02/2021",
                    "activity" : "Checked Price of Goods",
                    "details" : "The user navigated to the Pricing page of 'Anti-blue Reflection Glass' and checked the price of the Goods"
                },
                {
                    "date" : "2/02/2021",
                    "activity" : "User cancelled Order",
                    "details" : "User requested Cancellation of Order on 3 goods citing reasons that 'Payment Gateway not responding"
                }
            ]
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10983475,
            "user_status" : "Inactive",
            "last_seen" : "09/20/2021",
            "payment_status" : "Paid",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : [
                {
                    "date" : "12/02/2021",
                    "activity" : "Made a Sales Order",
                    "details" : "The User made a Purchase Order for 5 Goods and Payed via Card. The User also Paid for delivery via Courier services"
                },
                {
                    "date" : "10/02/2021",
                    "activity" : "Checked Price of Services",
                    "details" : "The user navigated to the Pricing page of 'Anti-blue Reflection Glass' and checked the price of the Goods"
                },
                {
                    "date" : "2/02/2021",
                    "activity" : "User cancelled Order",
                    "details" : "User requested Cancellation of Order on 3 goods citing reasons that 'Payment Gateway not responding"
                }
            ]
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10486475,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_date" : "12/12/2021",
            "payment_status" : "Unpaid",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : []
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10232195,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_status" : "Overdue",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : []
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybankz@gmail.com",
            "trx_id" : 10238475,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_status" : "Paid",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : [
                {
                    "date" : "12/02/2021",
                    "activity" : "Made a Purchase Order",
                    "details" : "The User made a Purchase Order for 5 Goods and Payed via Card. The User also Paid for delivery via Courier services"
                },
                {
                    "date" : "10/02/2021",
                    "activity" : "Checked Price of Goods",
                    "details" : "The user navigated to the Pricing page of 'Anti-blue Reflection Glass' and checked the price of the Goods"
                },
                {
                    "date" : "2/02/2021",
                    "activity" : "User cancelled Order",
                    "details" : "User requested Cancellation of Order on 3 goods citing reasons that 'Payment Gateway not responding"
                }
            ]
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10983475,
            "user_status" : "Inactive",
            "last_seen" : "09/20/2021",
            "payment_status" : "Paid",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : [
                {
                    "date" : "12/02/2021",
                    "activity" : "Made a Sales Order",
                    "details" : "The User made a Purchase Order for 5 Goods and Payed via Card. The User also Paid for delivery via Courier services"
                },
                {
                    "date" : "10/02/2021",
                    "activity" : "Checked Price of Services",
                    "details" : "The user navigated to the Pricing page of 'Anti-blue Reflection Glass' and checked the price of the Goods"
                },
                {
                    "date" : "2/02/2021",
                    "activity" : "User cancelled Order",
                    "details" : "User requested Cancellation of Order on 3 goods citing reasons that 'Payment Gateway not responding"
                }
            ]
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10486475,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_date" : "12/12/2021",
            "payment_status" : "Unpaid",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : []
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10232195,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_status" : "Overdue",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : []
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybankz@gmail.com",
            "trx_id" : 10238475,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_status" : "Paid",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : [
                {
                    "date" : "12/02/2021",
                    "activity" : "Made a Purchase Order",
                    "details" : "The User made a Purchase Order for 5 Goods and Payed via Card. The User also Paid for delivery via Courier services"
                },
                {
                    "date" : "10/02/2021",
                    "activity" : "Checked Price of Goods",
                    "details" : "The user navigated to the Pricing page of 'Anti-blue Reflection Glass' and checked the price of the Goods"
                },
                {
                    "date" : "2/02/2021",
                    "activity" : "User cancelled Order",
                    "details" : "User requested Cancellation of Order on 3 goods citing reasons that 'Payment Gateway not responding"
                }
            ]
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10983475,
            "user_status" : "Inactive",
            "last_seen" : "09/20/2021",
            "payment_status" : "Paid",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : [
                {
                    "date" : "12/02/2021",
                    "activity" : "Made a Sales Order",
                    "details" : "The User made a Purchase Order for 5 Goods and Payed via Card. The User also Paid for delivery via Courier services"
                },
                {
                    "date" : "10/02/2021",
                    "activity" : "Checked Price of Services",
                    "details" : "The user navigated to the Pricing page of 'Anti-blue Reflection Glass' and checked the price of the Goods"
                },
                {
                    "date" : "2/02/2021",
                    "activity" : "User cancelled Order",
                    "details" : "User requested Cancellation of Order on 3 goods citing reasons that 'Payment Gateway not responding"
                }
            ]
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10486475,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_date" : "12/12/2021",
            "payment_status" : "Unpaid",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : []
        },
        {
            "first_name" : "Promise",
            "last_name" : "Omos",
            "email" : "promzybank@gmail.com",
            "trx_id" : 10232195,
            "user_status" : "Active",
            "last_seen" : "09/20/2021",
            "payment_status" : "Overdue",
            "payment_date" : "12/12/2021",
            "due_date" : "24/12/2021",
            "amount" : 300,
            "currency" : "USD",
            "symbol" : "$",
            "user_activity" : []
        }
])

let pages = ref(1)
let startnum = ref(0)
let intervalnum = ref(10)
let endnum = ref(10)
const endss = computed(() => { return usingusersdata.value.length })
let limitnum = endss


// usingusersdata.value = usersdata;
// console.log(usingusersdata.value)

watchEffect(() => {

    console.log(startnum.value)
    console.log(endnum.value)

    if(showing.value != 'All'){
        usingusersdata.value = usersdata.filter(x => x.payment_status == showing.value)
        usingsliceddata.value = usingusersdata.value.slice(startnum.value, endnum.value)
    }else{
        usingusersdata.value = usersdata;
        usingsliceddata.value = usingusersdata.value.slice(startnum.value, endnum.value)
         
    }

    searcher();

    // console.log(usingusersdata.value.length)
})

watchEffect(() => {

    
    if(searchtext.value == 0){
        startnum.value = ((pages.value - 1) * intervalnum.value)
        endnum.value = intervalnum.value * pages.value > usingusersdata.value.length ? usingusersdata.value.length : intervalnum.value * pages.value
    }else{
        startnum.value = ((pages.value - 1) * intervalnum.value)
        endnum.value = intervalnum.value * pages.value > usingusersdata.value.length ? usingusersdata.value.length : intervalnum.value * pages.value
    }
    
})

let currentitemid = ref(0)
let currentlychecked = ref(false)

function currentlyOn(uid, event){
    currentitemid.value = uid
    currentlychecked.value = event.target.checked 
}


function searcher(){
    // alert("searching")

    // let query = this.searchQuery
    // console.log(searchtext.value)
    if(searchtext.value.length == 0){
        searchresult.value = []
        // usingusersdata.value = usersdata
    }else{
        let regsearch = new RegExp(`${searchtext.value}`, 'gi')
        // let regsearch2 = /searchtext.value/ig
        searchresult.value = usingusersdata.value.filter( (x) => x.first_name.match(regsearch) || x.last_name.match(regsearch) || x.email.match(regsearch) || x.payment_date.match(regsearch) || x.due_date.match(regsearch))
        usingsliceddata.value = searchresult.value.slice(startnum.value, endnum.value)
        // usingusersdata.value = searchresult.value
        console.log(searchresult.value)    
    }
    
}
</script>

<style lang="scss" scoped>

</style>