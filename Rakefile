require 'aws'
require 'yaml'
require 'logger'
require 'mime-types'

log = Logger.new(STDOUT)
log.level = Logger::INFO

config = YAML.load_file("config.yml")

desc "Deploy to S3."
task :deploy do

    s3 = Aws::S3.new(
      config[:aws][:access_key_id],
      config[:aws][:secret_access_key]
    )

    bucket = s3.bucket(config[:aws][:s3_bucket])

    log.info("Deploying")

    Dir["**/*"].each do |file|
      next if File.directory?(file)
      mime_type = MIME::Types.type_for(file).first
      log.debug("Uploading #{file} with Content-Type: #{mime_type}")
      headers = {'content-type' => mime_type}
      bucket.put(file, File.read(file), {}, 'public-read', headers)
    end

    log.info("Done!")
end
