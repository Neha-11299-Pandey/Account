# Account


When an Account is updated and the Website is filled in, update all the Profile field on all Contacts to:
Profile = Website + ‘/’ + First Letter of First Name + Last Name


trigger filedupdate on Account (after update) {
    set<id> idList = new set<id>();
    list<contact> conlist = new list<contact>();
    for(account acc : trigger.new){
        if(account.website != null){
            idlist.add(acc.id); 
        } 
        if(idlist.size()>0){ 
            for(contact c : [select id,firstname,lastname,Profile__c,account.website
                             from contact where accountid in :idlist]){
                                 c.Profile__c= c.account.website + '/' + c.firstname.left(1) + c.lastname;
                                 conlist.add(c);
                             } 
        }
        update conlist; 
    } 
}
