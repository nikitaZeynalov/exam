# exam

export default function showInfo(content) {
    const data = content.split('\n').slice(1, -1).filter(str => str.trim() !== '');
    console.log(`Count: ${data.length}`);

    const allCities = getValuesByIndex(data, 7);
    console.log(`Cities: ${[...new Set(allCities)].sort().join(', ')}`);

    const humidities = getValuesByIndex(data, 3).map(Number);
    console.log(`Humidity: Min: ${Math.min(...humidities)}, Max: ${Math.max(...humidities)}`);

    console.log(getInfoByMaxTemperature(data));

    console.log(findCityWithMaxAverageTemperature(data));
}

function getValuesByIndex(strs, index) {
    return strs.map(str => str.split(',')[index]);
}

function getValueByIndex(str, index) {
    return str.split(',')[index]
}

function getInfoByMaxTemperature(strs) {
    let maxTemperature = 0
    let index = 0
    for (let i = 0; i < strs.length; i++) {
        let currentTemperature = strs[i].split(',')[1];
        if (currentTemperature > maxTemperature) {
            maxTemperature = currentTemperature;
            index = i;
        }
    }
    return `HottestDay: ${getValueByIndex(strs[index], 0)} ${getValueByIndex(strs[index], 7)}`
}

function findCityWithMaxAverageTemperature(data) {
    let result = new Map();
    data.forEach(item => {
        let city = getValueByIndex(item, 7);
        let temperature = getValueByIndex(item, 1);
        if (result.has(city)) {
            result.get(city).push(parseInt(temperature, 10));
        } else {
            result.set(city, [parseInt(temperature, 10)]);
        }
    })

    let maxAverage = -Infinity;
    let cityWithMaxAverage = '';

    for (const [city, temperatures] of result) {
        const average = temperatures.reduce((sum, temp) => sum + temp, 0) / temperatures.length;

        if (average > maxAverage) {
            maxAverage = average;
            cityWithMaxAverage = city;
        }
    }
    return `HottestCity: ${cityWithMaxAverage}`;
}