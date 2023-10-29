<template>
    <div class="tourism-book p-4 bg-white rounded-1">
        <span
            class="p-2 d-block bg-secondary mb-4 bg-opacity-25 rounded-circle d-flex align-items-center justify-content-center"
            style="width: 40px; height: 40px; cursor: pointer;" @click="$emit('back-ward')" title="Back">
            <i :class="`fa-solid fa-chevron-${$i18n.locale === 'en' ? 'left' : 'right'}`"></i>
        </span>
        <div class="program-info text-center">
            <h2 class="mb-3 text-capitalize" v-if="programDetails.destination">
                {{ $i18n.locale === 'en' ? programDetails.destination.name_en : programDetails.destination.name }}
            </h2>
            <p class="text-muted fw-bold mb-3">
                {{ $i18n.locale === 'en' ? programDetails.program_title_english :
                    programDetails.program_title_arabic }}
            </p>
            <span class="text-white fw-bold d-block rounded-1 p-2 px-3 mx-auto">
                {{ programDetails.num_of_days }} {{ $t('programs.cardInfo.days') }} &
                {{ programDetails.num_of_nights }} {{ $t('programs.cardInfo.nights') }}
            </span>
        </div>
        <div class="flight-trip mt-5" v-if="programDetails.includes_flight == '1'">
            <h2 class="head text-center mb-5">
                {{ $t('programs.cardInfo.details') }}
            </h2>
            <div class="go-trip">
                <h3 class="mb-3">
                    {{ $t('programs.cardInfo.trip.go') }}
                </h3>
                <div class="info">
                    <div class="detail-label">
                        <label class="position-relative mb-1">
                            {{ $t('programs.cardInfo.trip.flightLine') }}
                        </label>
                        <span
                            class="p-2 px-3 rounded-1 bg-secondary text-center fw-bold text-capitalize bg-opacity-25 text-muted d-block"
                            v-if="flightTrip.flightAirLine">
                            {{ $i18n.locale === 'en' ? flightTrip.flightAirLine.name_en :
                                flightTrip.flightAirLine.name_ar }}
                        </span>
                    </div>
                    <div class="more-info d-flex align-items-start mt-3 gap-4 flex-lg-row flex-column">
                        <div class="flex-fill w-100">
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.from') }}
                                </label>
                                <span
                                    class="p-2 px-3 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block text-capitalize"
                                    v-if="flightTrip.from">
                                    {{ $i18n.locale === 'en' ? flightTrip.from.english_name :
                                        flightTrip.from.arabic_name }}
                                </span>
                            </div>
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.deptDateTime') }}
                                </label>
                                <div class="d-flex gap-2 w-100 flex-column flex-md-row">
                                    <span
                                        class="p-2 px-3 w-100 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                        {{ flightTrip.departureDate }}
                                    </span>
                                    <span
                                        class="p-2 px-3 w-100 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                        {{ flightTrip.departureTime }}
                                    </span>
                                </div>
                            </div>
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.allowedWeight') }}
                                </label>
                                <span class="p-2 px-3 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                    {{ +flightTrip.allowedWeight }}
                                </span>
                            </div>
                        </div>
                        <div class="flex-fill w-100">
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.to') }}
                                </label>
                                <span
                                    class="p-2 px-3 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block text-capitalize"
                                    v-if="flightTrip.to">
                                    {{ $i18n.locale === 'en' ? flightTrip.to.english_name :
                                        flightTrip.to.arabic_name }}
                                </span>
                            </div>
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.arrivalDateTime') }}
                                </label>
                                <div class="d-flex gap-2 w-100 flex-column flex-md-row">
                                    <span
                                        class="p-2 px-3 w-100 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                        {{ flightTrip.arrivDate22 }}
                                    </span>
                                    <span
                                        class="p-2 px-3 w-100 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                        {{ flightTrip.arrivalTime }}
                                    </span>
                                </div>
                            </div>
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.flightNum') }}
                                </label>
                                <span class="p-2 px-3 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                    {{ flightTrip.flightNumber }}
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="return-trip mt-5 flex-fill">
                <h3 class="mb-3">
                    {{ $t('programs.cardInfo.trip.return') }}
                </h3>
                <div class="info">
                    <div class="detail-label">
                        <label class="position-relative mb-1">
                            {{ $t('programs.cardInfo.trip.flightLine') }}
                        </label>
                        <span
                            class="p-2 px-3 rounded-1 bg-secondary text-center fw-bold text-capitalize bg-opacity-25 text-muted d-block"
                            v-if="flightTrip.flightAirLine">
                            {{ $i18n.locale === 'en' ? flightTrip.flightAirLine.name_en :
                                flightTrip.flightAirLine.name_ar }}
                        </span>
                    </div>
                    <div class="more-info d-flex align-items-start mt-3 gap-4 flex-lg-row flex-column">
                        <div class="flex-fill w-100">
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.from') }}
                                </label>
                                <span
                                    class="p-2 px-3 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block text-capitalize"
                                    v-if="flightTrip.returnFrom">
                                    {{ $i18n.locale === 'en' ? flightTrip.returnFrom.english_name :
                                        flightTrip.returnFrom.arabic_name }}
                                </span>
                            </div>
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.returnStartDateTime') }}
                                </label>
                                <div class="d-flex gap-2 w-100 flex-column flex-md-row">
                                    <span
                                        class="p-2 px-3 w-100 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                        {{ flightTrip.returnStartDate }}
                                    </span>
                                    <span
                                        class="p-2 px-3 w-100 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                        {{ flightTrip.returnEndDate1 }}
                                    </span>
                                </div>
                            </div>
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.allowedWeight') }}
                                </label>
                                <span class="p-2 px-3 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                    {{ +flightTrip.allowedWeightReturn }}
                                </span>
                            </div>
                        </div>
                        <div class="flex-fill w-100">
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.to') }}
                                </label>
                                <span
                                    class="p-2 px-3 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block text-capitalize"
                                    v-if="flightTrip.returnTo">
                                    {{ $i18n.locale === 'en' ? flightTrip.returnTo.english_name :
                                        flightTrip.returnTo.arabic_name }}
                                </span>
                            </div>
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.returnEndDateTime') }}
                                </label>
                                <div class="d-flex gap-2 w-100 flex-column flex-md-row">
                                    <span
                                        class="p-2 px-3 w-100 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                        {{ flightTrip.returnEndDate }}
                                    </span>
                                    <span
                                        class="p-2 px-3 w-100 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                        {{ flightTrip.returnEndDate2 }}
                                    </span>
                                </div>
                            </div>
                            <div class="detail-label my-2">
                                <label class="position-relative mb-1">
                                    {{ $t('programs.cardInfo.trip.flightNum') }}
                                </label>
                                <span class="p-2 px-3 rounded-1 bg-secondary fw-bold bg-opacity-25 text-muted d-block">
                                    {{ flightTrip.returnFlightNumber }}
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="programme-details mt-5">
            <h2 class="mb-3">
                {{ $t("programs.cardInfo.programDetails") }}
            </h2>
            <ul class="d-flex flex-column gap-2">
                <li v-if="typeof programDetails.program_details_english === 'object' && typeof programDetails.program_details_arabic === 'object'"
                    v-for="(item, index) in $i18n.locale === 'en' ? programDetails.program_details_english : programDetails.program_details_arabic "
                    :key="index" class="text-capitalize">
                    <i :class="`fa-solid fa-angles-${$route.params.lang === 'en' ? 'right' : 'left'}`"></i>
                    {{ item }}
                </li>
                <li v-else class="text-capitalize">
                    <i :class="`fa-solid fa-angles-${$route.params.lang === 'en' ? 'right' : 'left'}`"></i>
                    {{ $i18n.locale === 'en' ? programDetails.program_details_english :
                        programDetails.program_details_arabic }}
                </li>
            </ul>
        </div>
        <form>
            <div class="booking-box rounded-1 p-3 mt-5">
                <h3 class="text-center mb-2 position-relative">
                    {{ $t("buttons.bookNow") }}
                </h3>
                <h4 class="text-center mb-4 fw-bold">
                    ({{ $t("programs.search.leader") }})
                </h4>
                <div class="box d-flex gap-3 flex-lg-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.fName') }}:</span>
                        <input :class="`p-2 px-3 rounded-1 w-100 ${validation.first_name.$error ? 'error' : ''}`"
                            type="text" placeholder="First Name" v-model="bookingInfo.first_name">
                        <span v-if="validation.first_name.$error" class="error text-danger fst-italic d-block">
                            {{ validation.first_name.$errors[0].$validator === 'required' ? 'Field Required' : '' }}
                        </span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.lName') }}:</span>
                        <input :class="`p-2 px-3 rounded-1 w-100 ${validation.last_name.$error ? 'error' : ''}`" type="text"
                            placeholder="Last Name" v-model="bookingInfo.last_name">
                        <span v-if="validation.last_name.$error" class="error text-danger fst-italic d-block">
                            {{ validation.last_name.$errors[0].$validator === 'required' ? 'Field Required' : '' }}
                        </span>
                    </label>
                </div>
                <div class="box d-flex gap-3 mt-3 flex-lg-row flex-column">
                    <!-- <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.email') }}:</span>
                        <input :class="`p-2 px-3 rounded-1 w-100 ${validation.email.$error ? 'error' : ''}`" type="text"
                            placeholder="Email Address" v-model="bookingInfo.email">
                        <span v-if="validation.email.$error" class="error text-danger fst-italic d-block">
                            {{ validation.email.$errors[0].$validator === 'required' ?
                                'Field Required' :
                                'Invalid Email' }}
                        </span>
                    </label> -->
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.phone') }}:</span>
                        <input :class="`p-2 px-3 rounded-1 w-100 ${validation.phone.$error ? 'error' : ''}`" type="phone"
                            placeholder="Phone Number" v-model="bookingInfo.phone">
                        <span v-if="validation.phone.$error" class="error text-danger fst-italic d-block">
                            {{ validation.phone.$errors[0].$validator === 'required' ?
                                'Field Required' :
                                'Invalid Phone Number' }}
                        </span>
                    </label>
                </div>
                <div class="box d-flex gap-3 mt-3 flex-lg-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.nation') }}:</span>
                        <input :class="`p-2 px-3 rounded-1 w-100 ${validation.nationality.$error ? 'error' : ''}`"
                            type="text" placeholder="Nationality" v-model="bookingInfo.nationality">
                        <span v-if="validation.nationality.$error" class="error text-danger fst-italic d-block">
                            {{ validation.nationality.$errors[0].$validator === 'required' ? 'Field Required' : '' }}
                        </span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">
                            {{ $t('flightReservation.flightBook.passportNum') }}:
                        </span>
                        <input :class="`p-2 px-3 rounded-1 w-100 ${validation.passport_number.$error ? 'error' : ''}`"
                            type="text" placeholder="Passport Number" v-model="bookingInfo.passport_number">
                        <span v-if="validation.passport_number.$error" class="error text-danger fst-italic d-block">
                            {{ validation.passport_number.$errors[0].$validator === 'required' ? 'Field Required' :
                                'Passport can"t containe special characters' }}
                        </span>
                    </label>
                </div>
                <div class="box d-flex gap-3 mt-3 flex-lg-row flex-column" v-if="bookingInfo.personsForm.length > 0"
                    v-for="(item, index) in bookingInfo.personsForm" :key="index">
                    <label class="w-100 d-flex gap-2">
                        <div class="w-100">
                            <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.firstName') }} {{
                                `(${index + 2})` }}:</span>
                            <input
                                :class="`p-2 px-3 rounded-1 w-100 ${validation.personsForm.$each.$response.$data[index].firstName.$error ? 'error' : ''}`"
                                type="text" placeholder="First Name" v-model="item.firstName">
                            <span v-if="validation.personsForm.$each.$response.$data[index].firstName.$error"
                                class="error text-danger fst-italic d-block">
                                {{ validation.personsForm.$each.$response.$errors[index].firstName[0].$validator ===
                                    'required'
                                    ? 'Field Required' : '' }}
                            </span>
                        </div>
                        <div class="w-100">
                            <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.lastName') }} {{
                                `(${index + 2})` }}:</span>
                            <input
                                :class="`p-2 px-3 rounded-1 w-100 ${validation.personsForm.$each.$response.$data[index].lastName.$error ? 'error' : ''}`"
                                type="text" placeholder="Last Name" v-model="item.lastName">
                            <span v-if="validation.personsForm.$each.$response.$data[index].lastName.$error"
                                class="error text-danger fst-italic d-block">
                                {{ validation.personsForm.$each.$response.$errors[index].lastName[0].$validator ===
                                    'required'
                                    ? 'Field Required' : '' }}
                            </span>
                        </div>
                    </label>
                    <label class="w-100 d-flex gap-2">
                        <div class="w-100" v-if="item.type === 'MR' || item.type === 'MRS'">
                            <span class="d-block mb-1 fw-semibold">
                                {{ $t('flightReservation.flightBook.type') }} {{ `(${index + 2})` }}:
                            </span>
                            <select class="w-100 p-2 px-3 rounded-1" v-model="item.type">
                                <option value="MR">MR</option>
                                <option value="MRS">MRS</option>
                            </select>
                        </div>
                        <div class="w-100">
                            <span class="d-block mb-1 fw-semibold">
                                {{ $t('flightReservation.flightBook.type') }} {{ `(${index + 2})` }}:
                            </span>
                            <input
                                :class="`p-2 px-3 rounded-1 w-100 ${validation.personsForm.$each.$response.$data[index].type.$error ? 'error' : ''}`"
                                type="text" placeholder="Type" v-model="item.type" disabled>
                            <span v-if="validation.personsForm.$each.$response.$data[index].type.$error"
                                class="error text-danger fst-italic d-block">
                                {{ validation.personsForm.$each.$response.$errors[index].type[0].$validator ===
                                    'required' ?
                                    'Field Required' : '' }}
                            </span>
                        </div>
                    </label>
                </div>
                <div class="box my-5 d-flex gap-3 flex-column" v-if="searchInfo.rooms.length > 0">
                    <h3 class="text-center">Rooms</h3>
                    <div class="responsive-table overflow-auto">
                        <table class="w-100">
                            <thead>
                                <tr>
                                    <td class="text-center">#</td>
                                    <td>Room Type</td>
                                    <td>Child Reservation</td>
                                    <td>Child Count In Room</td>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in searchInfo.rooms" :key="index">
                                    <td class="text-center">
                                        {{ index + 1 }}
                                    </td>
                                    <td>
                                        {{ item.roomType === '1' ? 'Single Room' :
                                            item.roomType === '2' ?
                                                'Double Room' : 'Triple Room' }}
                                    </td>
                                    <td>
                                        {{ item.childReservation }}
                                    </td>
                                    <td>
                                        {{ item.childCount }}
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
                <div class="box my-5 d-flex gap-3 flex-column" v-if="allCities.length > 0">
                    <h3 class="text-center">Cities In Program</h3>
                    <div class="responsive-table overflow-auto">
                        <table class="w-100">
                            <thead>
                                <tr>
                                    <td class="text-center">#</td>
                                    <td>City</td>
                                    <td>Hotel Name</td>
                                    <td v-if="searchInfo.includeFlight == '1'">Check In</td>
                                    <td v-if="searchInfo.includeFlight == '1'">Check Out</td>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in allCities" :key="index">
                                    <td class="text-center">
                                        {{ index + 1 }}
                                    </td>
                                    <td v-if="item.city">
                                        {{ $i18n.locale === 'en' ? item.city.name_en : item.city.name }}
                                    </td>
                                    <td>
                                        {{ $i18n.locale === 'en' ? item.hotel_name_english : item.hotel_name_arabic }}
                                    </td>
                                    <td v-if="searchInfo.includeFlight == '1'">
                                        {{ item.registration_date }}
                                    </td>
                                    <td v-if="searchInfo.includeFlight == '1'">
                                        {{ item.departure_date }}
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
                <div class="box d-flex gap-3 mt-3 flex-md-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.meal') }}:</span>
                        <span class="p-2 px-3 rounded-1 w-100 bg-secondary bg-opacity-25 d-block w-100">-----</span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.transport') }}:</span>
                        <span class="p-2 px-3 rounded-1 w-100 bg-secondary bg-opacity-25 d-block w-100">-----</span>
                    </label>
                </div>
                <div class="box d-flex gap-3 mt-3 flex-md-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.adults') }}:</span>
                        <input class="p-2 px-3 rounded-1 w-100 bg-secondary bg-opacity-25" type="number" min="0"
                            v-model="persons.adults" disabled>
                    </label>
                    <label class="w-100" v-if="persons.children > 0">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.children') }}:</span>
                        <input class="p-2 px-3 rounded-1 w-100 bg-secondary bg-opacity-25" type="number" min="0"
                            v-model="persons.children" disabled>
                    </label>
                    <label class="w-100" v-if="persons.infants > 0">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.infants') }}:</span>
                        <input class="p-2 px-3 rounded-1 w-100 bg-secondary bg-opacity-25" type="number" min="0"
                            v-model="persons.infants" disabled>
                    </label>
                </div>
                <div class="box d-flex gap-3 mt-4 flex-md-row flex-column">
                    <label class="w-100" v-if="prices.has('1')">
                        <span class="d-block mb-1 fw-semibold">
                            {{ $t("programs.search.singlePrice") }}:
                        </span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ USDollar.format(programDetails.price_per_adult_individual) }}
                        </span>
                    </label>
                    <label class="w-100" v-if="prices.has('2')">
                        <span class="d-block mb-1 fw-semibold">
                            {{ $t("programs.search.doublePrice") }}:
                        </span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ USDollar.format(programDetails.price_per_adult_double) }}
                        </span>
                    </label>
                    <label class="w-100" v-if="prices.has('3')">
                        <span class="d-block mb-1 fw-semibold">
                            {{ $t("programs.search.triplePrice") }}:
                        </span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ USDollar.format(programDetails.price_per_adult_triple) }}
                        </span>
                    </label>
                </div>
                <div class="box d-flex gap-3 mt-4 flex-md-row flex-column">
                    <label class="w-100" v-if="prices.has('4')">
                        <span class="d-block mb-1 fw-semibold">
                            {{ $t("programs.search.childWithoutBedPrice") }}:
                        </span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ USDollar.format(programDetails.price_per_child_no_bed) }}
                        </span>
                    </label>
                    <label class="w-100" v-if="prices.has('5')">
                        <span class="d-block mb-1 fw-semibold">
                            {{ $t("programs.search.childWithBedPrice") }}:
                        </span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ USDollar.format(programDetails.price_per_child_with_bed) }}
                        </span>
                    </label>
                    <label class="w-100" v-if="prices.has('6')">
                        <span class="d-block mb-1 fw-semibold">
                            {{ $t("programs.search.infantPrice") }}:
                        </span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ USDollar.format(programDetails.price_per_infant) }}
                        </span>
                    </label>
                </div>
                <div class="box d-flex gap-3 flex-column mt-3">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.total') }}:</span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ USDollar.format(bookingInfo.total) }}
                        </span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.tax') }}:</span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ +bookingInfo.tax }}%
                        </span>
                    </label>
                    <label class="w-100 mt-3">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.agentDiscount') }}:</span>
                        <span class="d-block p-2 px-3 rounded-1 bg-secondary bg-opacity-25">
                            {{ USDollar.format(bookingInfo.agent_discount || 0) }}
                        </span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('flightReservation.flightBook.net') }}:</span>
                        <span class="mb-1 bg-secondary bg-opacity-25 w-100 d-block rounded-1 p-2 px-3">
                            {{ USDollar.format(bookingInfo.net_total) }}
                        </span>
                    </label>
                    <button class="butn rounded-1 p-2 px-3 text-uppercase mt-3" @click.prevent="submission">
                        {{ $t('buttons.confirm') }}
                    </button>
                </div>
            </div>
        </form>
        <div :class="`alert alert-success alert-dismissible text-${$i18n.locale === 'en' ? 'start' : 'end'} position-fixed`"
            role="alert">
            <div class="d-flex align-items-center gap-2">
                <i class="fa-solid fa-circle-check fs-5"></i>
                {{ $t('flightReservation.bookMessage') }}
            </div>
            <span class="d-block mt-1">
                {{ `${$t('flightReservation.bookingCode')}: ${randomCode}` }}
            </span>
            <span class="d-block mt-1">
                {{ `${$t('flightReservation.yourBalance')}: ${USDollar.format(updatedBalance)}` }}
            </span>
            <button @click="removeAlert('success')" type="button" class="btn-close"></button>
        </div>
        <div id="alert-1" class="alert alert-danger alert-dismissible text-center position-fixed" role="alert">
            <div class="d-flex align-items-center gap-2">
                <i class="fa-solid fa-circle-xmark fs-5"></i>
                {{ $t('flightReservation.noBalance') }}
            </div>
            <button @click="removeAlert('danger')" type="button" class="btn-close"></button>
        </div>
        <div id="alert-2" class="alert alert-danger alert-dismissible text-center position-fixed" role="alert">
            <div class="d-flex align-items-center gap-2">
                <i class="fa-solid fa-circle-xmark fs-5"></i>
                There Is No Tickets Enough
            </div>
            <button @click="removeAlert('danger')" type="button" class="btn-close"></button>
        </div>
        <Loader v-if="loading"></Loader>
    </div>
</template>
<script setup>
import axios from 'axios';
import { ref, onMounted, computed, watch } from 'vue'
import { useVuelidate } from "@vuelidate/core";
import { required, email, numeric, alphaNum, helpers } from "@vuelidate/validators";
import Loader from '../Loader.vue';
import { useRoute } from 'vue-router';
import i18n from '../../i18n';
import router from '../../router';




const prices = ref(new Set())
const social = ref([])
const userInfo = ref({})
const loading = ref(false)
const route = useRoute()
const props = defineProps(['programId', 'searchResults', 'searchInfo', 'cities'])
const programDetails = ref({})
const allCities = ref([])
const randomCode = ref('')
const updatedBalance = ref('')
const persons = ref({
    infants: props.searchInfo.no_infants,
    children: props.searchInfo.no_children,
    adults: props.searchInfo.no_adults
})
const flightTrip = ref({})
const bookingInfo = ref({
    first_name: '',
    last_name: '',
    email: '',
    phone: '',
    nationality: '',
    passport_number: '',
    personsForm: [],
    adults_count: computed(() => persons.value.infants + persons.value.children + persons.value.adults),
    tax: computed(() => programDetails.value.tax),
    total: 0,
    agent_discount: computed(() => (userInfo.value.discount * (bookingInfo.value.total + ((bookingInfo.value.tax / 100) * bookingInfo.value.total))) / 100),
    net_total: computed(() => ((bookingInfo.value.total + ((bookingInfo.value.tax / 100) * bookingInfo.value.total)) - bookingInfo.value.agent_discount))
})


const rules = {
    first_name: { required },
    last_name: { required },
    // email: { required, email },
    phone: { required, numeric },
    nationality: { required },
    passport_number: { required, alphaNum },
    personsForm: {
        $each: helpers.forEach({
            firstName: { required },
            lastName: { required },
            type: { required },
        })
    }
}
const validation = useVuelidate(rules, bookingInfo)

// Formating Balanc 
const USDollar = Intl.NumberFormat("en-US", {
    currency: "USD",
    style: "currency"
})


watch(i18n.global.locale, newVal => {
    if (bookingInfo.value.adults_count > 1) {
        bookingInfo.value.personsForm.forEach((el, i) => {
            el.type = persons.value.adults > i + 1 ?
                'MR' :
                persons.value.adults + persons.value.children > i + 1 ?
                    'Children' :
                    'Infant'
        })
    }
})


const submission = async () => {
    validation.value.$validate();
    if (!validation.value.$error) {
        const userId = JSON.parse(localStorage.getItem("login"))
        bookingInfo.value.email = userId.id

        loading.value = true
        const formData = new FormData()
        formData.append("destination", [...prices.value].join(','))
        formData.append("brogram_id", props.programId)
        formData.append("country", props.searchInfo.country.toString())
        formData.append("city", props.searchInfo.city.toString())
        formData.append("include_flight", props.searchInfo.includeFlight)
        formData.append("flightNumber", flightTrip.value.id.toString())
        formData.append("firstName", bookingInfo.value.first_name)
        formData.append("lastName", bookingInfo.value.last_name)
        formData.append("email", bookingInfo.value.email)
        formData.append("phoneNumber", bookingInfo.value.phone)
        formData.append("nationality", bookingInfo.value.nationality)
        formData.append("passportNumber", bookingInfo.value.passport_number)
        formData.append("numberOfChildren", persons.value.children)
        formData.append("numberOfInfants", persons.value.infants)
        formData.append("numberOfAdults", persons.value.adults)
        formData.append("total", bookingInfo.value.total)
        formData.append("tax", bookingInfo.value.tax)
        formData.append("net", bookingInfo.value.net_total)
        if (bookingInfo.value.personsForm.length > 0) {
            for (let i = 0; i < bookingInfo.value.personsForm.length; i++) {
                formData.append(`person${2 + i}`, `${bookingInfo.value.personsForm[i].firstName},${bookingInfo.value.personsForm[i].lastName}-${bookingInfo.value.personsForm[i].type}`)
            }
        }

        if (userInfo.value.balance > bookingInfo.value.net_total) {
            if (props.searchInfo.includeFlight == '1') {
                if (flightTrip.value.allowReturn != 1 && flightTrip.value.numTickets > bookingInfo.value.adults_count) {
                    const tickets = new FormData()
                    tickets.append("ticket_id", flightTrip.value.id)
                    tickets.append("new_number_of_tickets", flightTrip.value.numTickets - bookingInfo.value.adults_count)
                    await axios.post("https://api.seasonsge.com/ticket-out", tickets)
                        .then(data => {
                            console.log(data);
                        })
                } else if (flightTrip.value.allowReturn == 1 && flightTrip.value.numTickets > bookingInfo.value.adults_count && flightTrip.value.numReturnTickets > bookingInfo.value.adults_count) {
                    const tickets = new FormData()
                    tickets.append("ticket_id", flightTrip.value.id)
                    tickets.append("new_number_of_tickets", flightTrip.value.numTickets - bookingInfo.value.adults_count)
                    await axios.post("https://api.seasonsge.com/ticket-out", tickets)
                        .then(data => {
                            console.log(data);
                        })
                    const returnTickets = new FormData()
                    returnTickets.append("ticket_id", flightTrip.value.id)
                    returnTickets.append("new_number_of_tickets", flightTrip.value.numReturnTickets - bookingInfo.value.adults_count)
                    await axios.post("https://api.seasonsge.com/return_ticket", returnTickets)
                        .then(data => {
                            console.log(data);
                        })
                    await axios.post("https://api.seasonsge.com/program-booking", formData)
                        .then(response => {
                            console.log(response);
                            updatedBalance.value = parseFloat(userInfo.value.balance) - parseFloat(bookingInfo.value.net_total)
                            const balance = new FormData()

                            balance.append("name", userInfo.value.name)
                            balance.append("email", userInfo.value.email)
                            balance.append("password", userInfo.value.password)
                            balance.append("type", userInfo.value.type)
                            balance.append("id", userInfo.value.id)
                            balance.append("discount", userInfo.value.discount)
                            balance.append("balance", updatedBalance.value)

                            axios.post("https://api.seasonsge.com/user-edit", balance)
                                .then(userResponse => {
                                    if (userResponse.data.success) {
                                        randomCode.value = response.data.code
                                        document.querySelector(".alert-success").classList.add("active")
                                        loading.value = false

                                        const allRooms = new FormData()
                                        props.searchInfo.rooms.forEach(el => {
                                            allRooms.append("id_hotel", randomCode.value)
                                            allRooms.append("room_type", el.roomType)
                                            allRooms.append("child_room", `${el.childReservation},${el.childCount}`)
                                            axios.post("https://api.seasonsge.com/program-room", allRooms)
                                                .then(data => {
                                                    console.log(data);
                                                })
                                        })

                                        axios.get(`https://api.seasonsge.com/br-rr?id=${userId.id}`)
                                            .then(data => {
                                                const bookId = data.data.pop()
                                                setTimeout(() => {
                                                    router.push({
                                                        name: "Agents Programs Checkout",
                                                        params: { lang: i18n.global.locale.value, id: bookId.id, with: 3 }
                                                    })
                                                }, 1500)
                                            })
                                    }
                                })
                        })
                } else {
                    console.log("There Is No Tickets Enough");
                    document.querySelector("#alert-2").classList.add("active")
                    loading.value = false
                }
            } else {
                await axios.post("https://api.seasonsge.com/program-booking", formData)
                    .then(response => {
                        console.log(response);
                        updatedBalance.value = parseFloat(userInfo.value.balance) - parseFloat(bookingInfo.value.net_total)
                        const balance = new FormData()

                        balance.append("name", userInfo.value.name)
                        balance.append("email", userInfo.value.email)
                        balance.append("password", userInfo.value.password)
                        balance.append("type", userInfo.value.type)
                        balance.append("id", userInfo.value.id)
                        balance.append("discount", userInfo.value.discount)
                        balance.append("balance", updatedBalance.value)

                        axios.post("https://api.seasonsge.com/user-edit", balance)
                            .then(userResponse => {
                                if (userResponse.data.success) {
                                    randomCode.value = response.data.code
                                    document.querySelector(".alert-success").classList.add("active")
                                    loading.value = false

                                    const allRooms = new FormData()
                                    props.searchInfo.rooms.forEach(el => {
                                        allRooms.append("id_hotel", randomCode.value)
                                        allRooms.append("room_type", el.roomType)
                                        allRooms.append("child_room", `${el.childReservation},${el.childCount}`)
                                        axios.post("https://api.seasonsge.com/program-room", allRooms)
                                            .then(data => {
                                                console.log(data);
                                            })
                                    })

                                    axios.get(`https://api.seasonsge.com/br-rr?id=${userId.id}`)
                                        .then(data => {
                                            const bookId = data.data.pop()
                                            setTimeout(() => {
                                                router.push({
                                                    name: "Agents Programs Checkout",
                                                    params: { lang: i18n.global.locale.value, id: bookId.id, with: 3 }
                                                })
                                            }, 1500)
                                        })
                                }
                            })
                    })
            }
        } else {
            document.querySelector("#alert-1").classList.add("active")
            setTimeout(() => {
                document.querySelector("#alert-1").classList.remove("active")
            }, 3000)
            loading.value = false
        }
    }
};

const removeAlert = (type) => {
    document.querySelector(`.alert-${type}`).classList.remove("active")
}


onMounted(async () => {
    props.searchResults.filter(el => {
        if (el.id == props.programId) {
            programDetails.value = el
        }
    })
    await axios.get("https://api.seasonsge.com/cities-view")
        .then(data => {
            programDetails.destination = data.data.filter(ele => ele.id == programDetails.value.return_airline)[0]
        })
    await axios.get("https://api.seasonsge.com/country-view")
        .then(data => {
            programDetails.value.country = data.data.filter(
                (ele) => ele.id == props.searchInfo.country
            )[0];
        })
    if (programDetails.value.includes_flight == '1') {
        axios.get("https://api.seasonsge.com/flights?all").then(data => {
            flightTrip.value = data.data.filter(ele => {
                if (ele.flightNumber == programDetails.value.flight_number &&
                    ele.departureDate >= new Date().toLocaleDateString("en-CA")) {
                    return ele
                }
            })[0]
            axios.get("https://api.seasonsge.com/airlines-view")
                .then(data => {
                    if (data.data.success) {
                        flightTrip.value['flightAirLine'] = data.data.airlines.filter(airLine => airLine.id == flightTrip.value.flightLine)[0]
                    }
                })
            axios.get("https://api.seasonsge.com/viewAirports")
                .then(data => {
                    if (data.data.success) {
                        flightTrip.value['from'] = data.data.data.filter(element => element.id == flightTrip.value.fromAirport)[0]
                        flightTrip.value['to'] = data.data.data.filter(element => element.id == flightTrip.value.toAirport)[0]
                        flightTrip.value['returnFrom'] = data.data.data.filter(element => element.id == flightTrip.value.departureToReturn)[0]
                        flightTrip.value['returnTo'] = data.data.data.filter(element => element.id == flightTrip.value.arrivalFromReturn)[0]
                    }
                })
        })
    }
    if (bookingInfo.value.adults_count > 1) {
        for (let i = 0; i < bookingInfo.value.adults_count - 1; i++) {
            bookingInfo.value.personsForm.push({
                firstName: '',
                lastName: '',
                type: persons.value.adults > i + 1 ?
                    'MR' :
                    persons.value.adults + persons.value.children > i + 1 ?
                        'Children' :
                        'Infant'
            })
        }
    }
})


onMounted(async () => {
    await axios.get(`https://api.seasonsge.com/pr-data?id=${props.programId}`)
        .then(data => {
            if (typeof data.data === 'object') {
                allCities.value = data.data
                axios.get("https://api.seasonsge.com/cities-view")
                    .then(data => {
                        allCities.value.forEach((el) => {
                            el.city = data.data.filter((ele) => ele.id == el.city_name)[0];
                        });
                    })
            }
        })
    props.searchInfo.rooms.forEach((el) => {
        if (el.roomType == "1") {
            prices.value.add("1")
            bookingInfo.value.total += (+programDetails.value.price_per_adult_individual);
        } else if (el.roomType == "2") {
            prices.value.add("2")
            bookingInfo.value.total += (+programDetails.value.price_per_adult_double * 2);
        } else if (el.roomType == "3") {
            prices.value.add("3")
            bookingInfo.value.total += (+programDetails.value.price_per_adult_triple * 3);
        }
        if (el.childReservation === "child without bed") {
            prices.value.add("4")
            bookingInfo.value.total += (+programDetails.value.price_per_child_no_bed * el.childCount);
        } else if (el.childReservation === "child with bed") {
            prices.value.add("5")
            bookingInfo.value.total += (+programDetails.value.price_per_child_with_bed * el.childCount);
        }
    });
    if (props.searchInfo.no_infants > 0) {
        prices.value.add("6")
    }
    bookingInfo.value.total += (+programDetails.value.price_per_infant * props.searchInfo.no_infants)
    await axios.get("https://api.seasonsge.com/usersview").then((data) => {
        userInfo.value = data.data.filter((el) => el.id == route.params.userId)[0];
    });
    await axios.get("https://api.seasonsge.com/info")
        .then(data => {
            social.value = data.data[0]
        })
})

</script>
<style lang="scss" scoped>
.tourism-book {

    input,
    select {
        outline: none;
        border: 1px solid darkgray;

        &.error {
            border: 1px solid #dc3545;
        }
    }

    .program-info {
        h2 {
            color: var(--blue-color);
        }

        span {
            background-color: var(--orange-color);
            width: fit-content;
        }
    }

    .flight-trip {
        .head {
            position: relative;

            &::before {
                content: "";
                position: absolute;
                width: 15%;
                left: 50%;
                transform: translateX(-50%);
                height: 3px;
                bottom: -10px;
                border-radius: 50px;
                background-color: var(--blue-color);
            }
        }
    }

    .programme-details {

        h2 {
            color: var(--orange-color);
        }

        ul {

            li {
                display: flex;
                gap: 10px;
                font-weight: 500;

                i {
                    color: var(--blue-color);
                    position: relative;
                    top: 5px;
                }
            }
        }
    }

    .booking-box {
        box-shadow: 0 10px 15px rgba(0, 0, 0, 0.090);
        border: 1px solid var(--orange-color);

        h3 {
            &::before {
                left: 50%;
                transform: translateX(-50%);
                width: 8%;
            }
        }
    }

    table {
        min-width: 929px;

        td {
            padding: 10px;
            border: 1px solid darkgray;
        }

        thead {
            td {
                background-color: darkgray;
                color: white;
                font-weight: 500;
            }
        }

        tbody {
            tr {
                transition: 0.3s;

                td {
                    text-transform: capitalize;
                }

                &:hover {
                    background-color: #f3f3f3;
                }
            }

            .del-butn {
                transition: 0.3s;

                &:hover {
                    background-color: rgb(194, 194, 194) !important;
                }
            }
        }
    }

    .alert {
        top: -25%;
        left: 50%;
        transform: translateX(-50%);
        transition: 0.3s;
        z-index: 555555555;

        &.active {
            top: 5%;
        }

        button {
            box-shadow: none;
            outline: none;
        }
    }

    @media (max-width: 767px) {
        padding: 20px 15px !important;

        .alert {
            width: 90%;
            text-align: start !important;
            font-size: 12px;

            i {
                font-size: 16px !important;
            }
        }

        .booking-box {
            padding: 20px 10px !important;
        }
    }

}
</style>