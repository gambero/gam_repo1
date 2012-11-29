gam_repo1
=========
#!/usr/bin/perl                                                                                                                                                                 

use strict;
use warnings;
use LWP::UserAgent;
use HTTP::Request::Common qw(POST);

my $ua = LWP::UserAgent->new;
$ua->timeout(15);
$ua->agent('Mozilla');

my $referer = 'http://www.phosphosite.org/sequenceLogoAction.do';

my %params=(
    sequenceFile => ["./A2.txt"],
    Title => 'A2.txt',
    algorithm => 'freqTimesLogFold',
    firstnum => '-6',
    Generate => 'submit'
    );

my $req = POST(
    $referer,
    Content_Type => 'multipart/form-data',
    Content => [%params]
    );

my $response = $ua->request($req);
my $url="http://www.phosphosite.org/sequenceLogoSubmitAction.do";
$response = $ua->request(HTTP::Request->new(GET=>$url));

if ($response->is_success) {
    print $response->content,"\n";
}
else{
    print $response->status_line . "\n";
}

