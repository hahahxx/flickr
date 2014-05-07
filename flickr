#   An easy interface to the Flickr photo service. By Xinxin Huang
# TODO:
require 'cgi'
require 'net/http'
require 'xmlsimple'

class Flickr

  def initialize(api_key=nil, email=nil, password=nil)
    @api_key = api_key
    @host = 'http://flickr.com'
    @api = '/services/rest'
    login(email, password) if email and password
  end

  def request(method, *params)
    response =
      XmlSimple.xml_in(http_get(request_url(method, params)),
                       { 'ForceArray' => false })
    raise response['err']['msg'] if response['stat'] != 'ok'
    response
  end

  def getHttp(url)
    Net::HTTP.get_response(URI.parse(url)).body.to_s
  end

  def login(email='', password='')
    @email = email
    @password = password
    user = request('test.login')['user'] rescue fail
    @user = User.new(user['id'], @api_key)
  end

  def findByUrl(url)
    response = urls_lookupUser('url'=>url) rescue urls_lookupGroup('url'=>url) rescue nil
    (response['user']) ? User.new(response['user']['id'], @api_key) : Group.new(response['group']['id'], @api_key) unless response.nil?
  end

end
