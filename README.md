@Entity
@Table(name = "BRANCH_MASTER")
public class BranchMaster {

    @Id
    @Column(name = "BRANCHNO")
    String branchno;

    @Column(name = "REGION_NO")
    String regionno;

    @Column(name = "BR_NAME")
    String brname;

    @Column(name = "ADDRESS_1")
    String address1;

    @Column(name = "ADDRESS_2")
    String address2;

    @Column(name = "ADDRESS_3")
    String address3;

    @Column(name = "POST_CODE")
    String postcode;

    @Column(name = "PHONE_NO")
    String phoneno;

    @Column(name = "NETWORK_CODE")
    String networkcode;

    @Column(name = "STD_CODE")
    String stdcode;

    @Column(name = "CIRCLE_CODE")
    String circlecode;

    @Column(name = "MODULE_CODE")
    String modulecode;

    @Column(name = "REGION_CODE")
    String regioncode;

    @Column(name = "VOIP_PHONE_NO")
    String voipphoneno;

    @Column(name = "MAN_RES_PHONE")
    String manresphone;

    @Column(name = "MAN_MOBI_PHONE")
    String manmobiphone;

    @Column(name = "INST")
    String inst;

    @Column(name = "MANAGERS_NAME")
    String managersname;

    @Column(name = "CRS_AUDITABLE")
    String crsauditable;
