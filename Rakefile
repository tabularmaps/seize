require 'hocon'
require 'csv'
require 'iso_country_codes'

$conf = Hocon.load('hocon/seize.conf')

desc ''
task :sdgs do
  print "seriesCode,iso2cd,year,value\n"
  $conf['seriesCodes'].each {|seriesCode|
    $stderr.print "#{seriesCode}\n"
    cmd = "curl -X POST --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/octet-stream' -d 'seriesCodes=#{seriesCode}&timePeriodStart=2018' 'https://unstats.un.org/SDGAPI/v1/sdg/Series/DataCSV'"
    $stderr.print "#{cmd}\n"
    CSV.parse(IO.popen(cmd), headers: true).each {|r|
      begin
        print [r[3], IsoCountryCodes.find(r[5].to_i).alpha2.downcase, r[7].to_i, Float(r[8])].join(','), "\n"
      rescue
      end
    }
  }
end

